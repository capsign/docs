# CMX Protocol Deployment Guide

Complete guide for deploying the CMX Protocol smart contracts to various networks.

## Prerequisites

### Development Environment

- **Foundry v1.1.0+** (required for diamond pattern support)
- **Node.js 18+** and npm/yarn
- **Git** for version control
- **Base/Ethereum wallet** with sufficient gas funds

### Network Access

- **Base Sepolia** - Testnet deployment
- **Base Mainnet** - Production deployment
- **Ethereum Sepolia** - Cross-chain testing
- **Ethereum Mainnet** - Cross-chain production

### Required Tools

```bash
# Install Foundry
curl -L https://foundry.paradigm.xyz | bash
foundryup

# Verify installation
forge --version
cast --version
anvil --version
```

## Environment Setup

### 1. Clone and Install

```bash
git clone https://github.com/capsign/protocol.git
cd protocol
forge install
npm install
```

### 2. Environment Configuration

```bash
# Copy environment template
cp .env.example .env
```

### 3. Configure Environment Variables

```bash
# .env file configuration

# Network Configuration
BASE_SEPOLIA_RPC_URL=https://sepolia.base.org
BASE_MAINNET_RPC_URL=https://mainnet.base.org
ETHEREUM_SEPOLIA_RPC_URL=https://ethereum-sepolia.blockpi.network/v1/rpc/public
ETHEREUM_MAINNET_RPC_URL=https://ethereum.blockpi.network/v1/rpc/public

# Deployment Keys (Use different keys for different environments)
DEPLOYER_PRIVATE_KEY=0x...  # Deployer account private key
ADMIN_PRIVATE_KEY=0x...     # Protocol admin private key
OPERATOR_PRIVATE_KEY=0x...  # Operational account private key

# Verification Keys
BASESCAN_API_KEY=...        # For contract verification
ETHERSCAN_API_KEY=...       # For Ethereum verification

# Oracle Configuration
CHAINLINK_PRICE_FEED=0x... # ETH/USD price feed
CUSTOM_ORACLE_URL=https://... # Custom price oracle

# External Services
IPFS_GATEWAY_URL=https://ipfs.io/ipfs/
ATTESTATION_SERVICE_URL=https://...

# Protocol Configuration
INITIAL_ADMIN=0x...         # Initial protocol admin address
PROTOCOL_FEE_RATE=250       # 2.5% (in basis points)
MIN_INVESTMENT=1000000      # $1M minimum investment (in wei/smallest unit)
```

## Deployment Architecture

### Core Infrastructure Deployment Order

1. **Diamond Infrastructure**

   - DiamondCutFacet
   - DiamondLoupeFacet
   - OwnershipFacet
   - FacetRegistry

2. **Access Management**

   - GlobalAccessManager
   - Role configuration

3. **Core Registries**

   - AttestationRegistry
   - DocumentRegistry
   - PriceOracle

4. **Factory Contracts**

   - WalletFactory
   - AssetFactory
   - FundFactory
   - OfferingFactory

5. **System Contracts**
   - SubscriptionManager
   - CMXPaymaster
   - LedgerFactory

## Step-by-Step Deployment

### 1. Deploy Core Diamond Infrastructure

```bash
# Deploy diamond infrastructure
forge script script/00_DeployDiamondInfrastructure.s.sol \
  --rpc-url $BASE_SEPOLIA_RPC_URL \
  --private-key $DEPLOYER_PRIVATE_KEY \
  --broadcast \
  --verify \
  --etherscan-api-key $BASESCAN_API_KEY
```

**Expected Output**:

```
✅ DiamondCutFacet deployed: 0x1234...
✅ DiamondLoupeFacet deployed: 0x5678...
✅ OwnershipFacet deployed: 0x9abc...
✅ FacetRegistry deployed: 0xdef0...
```

### 2. Deploy Access Management

```bash
# Deploy access management system
forge script script/01_DeployAccessManagement.s.sol \
  --rpc-url $BASE_SEPOLIA_RPC_URL \
  --private-key $DEPLOYER_PRIVATE_KEY \
  --broadcast \
  --verify \
  --etherscan-api-key $BASESCAN_API_KEY
```

### 3. Deploy Core Registries

```bash
# Deploy registry contracts
forge script script/02_DeployRegistries.s.sol \
  --rpc-url $BASE_SEPOLIA_RPC_URL \
  --private-key $DEPLOYER_PRIVATE_KEY \
  --broadcast \
  --verify \
  --etherscan-api-key $BASESCAN_API_KEY
```

### 4. Deploy Factory Contracts

```bash
# Deploy all factory contracts
forge script script/03_DeployFactories.s.sol \
  --rpc-url $BASE_SEPOLIA_RPC_URL \
  --private-key $DEPLOYER_PRIVATE_KEY \
  --broadcast \
  --verify \
  --etherscan-api-key $BASESCAN_API_KEY
```

### 5. Deploy System Contracts

```bash
# Deploy system contracts
forge script script/04_DeploySystemContracts.s.sol \
  --rpc-url $BASE_SEPOLIA_RPC_URL \
  --private-key $DEPLOYER_PRIVATE_KEY \
  --broadcast \
  --verify \
  --etherscan-api-key $BASESCAN_API_KEY
```

### 6. Initialize System Configuration

```bash
# Initialize protocol configuration
forge script script/05_InitializeProtocol.s.sol \
  --rpc-url $BASE_SEPOLIA_RPC_URL \
  --private-key $ADMIN_PRIVATE_KEY \
  --broadcast
```

## Post-Deployment Configuration

### 1. Access Control Setup

```bash
# Grant necessary roles to operators
cast send $GLOBAL_ACCESS_MANAGER \
  "grantRole(bytes32,address,uint32)" \
  $OPERATOR_ROLE \
  $OPERATOR_ADDRESS \
  0 \
  --rpc-url $BASE_SEPOLIA_RPC_URL \
  --private-key $ADMIN_PRIVATE_KEY

# Grant compliance role
cast send $GLOBAL_ACCESS_MANAGER \
  "grantRole(bytes32,address,uint32)" \
  $COMPLIANCE_ROLE \
  $COMPLIANCE_ADDRESS \
  0 \
  --rpc-url $BASE_SEPOLIA_RPC_URL \
  --private-key $ADMIN_PRIVATE_KEY
```

### 2. Attestation Schema Setup

```bash
# Create KYC attestation schema
cast send $ATTESTATION_REGISTRY \
  "createSchema(string,bool)" \
  "address verified, uint256 verificationLevel, uint256 expiryDate" \
  true \
  --rpc-url $BASE_SEPOLIA_RPC_URL \
  --private-key $COMPLIANCE_PRIVATE_KEY
```

### 3. Price Oracle Configuration

```bash
# Set price feeds for supported assets
cast send $PRICE_ORACLE \
  "addPriceFeed(address,address)" \
  $CMX_TOKEN_ADDRESS \
  $CMX_PRICE_FEED_ADDRESS \
  --rpc-url $BASE_SEPOLIA_RPC_URL \
  --private-key $OPERATOR_PRIVATE_KEY
```

### 4. Subscription Tier Setup

```bash
# Configure subscription tiers
cast send $SUBSCRIPTION_MANAGER \
  "createTier(string,uint256,uint256,bool)" \
  "Professional" \
  $(cast --to-wei 100 ether) \
  86400 \
  true \
  --rpc-url $BASE_SEPOLIA_RPC_URL \
  --private-key $ADMIN_PRIVATE_KEY
```

## Verification & Testing

### 1. Contract Verification

Verify all deployed contracts on block explorers:

```bash
# Verify factory contracts
forge verify-contract \
  --chain-id 84532 \
  --num-of-optimizations 200 \
  --compiler-version v0.8.20 \
  $ASSET_FACTORY_ADDRESS \
  src/Tokenization/factory/AssetFactory.sol:AssetFactory \
  --etherscan-api-key $BASESCAN_API_KEY
```

### 2. Integration Testing

```bash
# Run integration tests against deployed contracts
forge test --match-contract IntegrationTest \
  --rpc-url $BASE_SEPOLIA_RPC_URL \
  --fork-block-number latest
```

### 3. System Health Check

```bash
# Check system status
forge script script/HealthCheck.s.sol \
  --rpc-url $BASE_SEPOLIA_RPC_URL \
  --private-key $OPERATOR_PRIVATE_KEY
```

## Network-Specific Configurations

### Base Sepolia (Testnet)

```bash
# Network ID: 84532
# Block Explorer: https://sepolia.basescan.org
# Faucet: https://faucet.quicknode.com/base/sepolia

# Testnet-specific settings
NETWORK=base-sepolia
CHAIN_ID=84532
BLOCK_EXPLORER_URL=https://sepolia.basescan.org
```

### Base Mainnet (Production)

```bash
# Network ID: 8453
# Block Explorer: https://basescan.org
# Bridge: https://bridge.base.org

# Production settings
NETWORK=base-mainnet
CHAIN_ID=8453
BLOCK_EXPLORER_URL=https://basescan.org
```

### Ethereum Sepolia (Cross-chain Testing)

```bash
# Network ID: 11155111
# Block Explorer: https://sepolia.etherscan.io
# Faucet: https://sepoliafaucet.com

# Cross-chain bridge testing
NETWORK=ethereum-sepolia
CHAIN_ID=11155111
BLOCK_EXPLORER_URL=https://sepolia.etherscan.io
```

## Security Considerations

### Multi-Signature Setup

1. **Deploy Multi-Sig Wallet**

```bash
# Deploy Gnosis Safe or similar multi-sig
# Recommend 3/5 or 5/7 configuration for production
```

2. **Transfer Ownership**

```bash
# Transfer protocol ownership to multi-sig
cast send $GLOBAL_ACCESS_MANAGER \
  "transferOwnership(address)" \
  $MULTISIG_ADDRESS \
  --rpc-url $BASE_MAINNET_RPC_URL \
  --private-key $ADMIN_PRIVATE_KEY
```

### Time Delays

Configure appropriate time delays for sensitive operations:

```bash
# Set 24-hour delay for critical functions
cast send $GLOBAL_ACCESS_MANAGER \
  "setRoleDelay(bytes32,uint32)" \
  $ADMIN_ROLE \
  86400 \
  --rpc-url $BASE_MAINNET_RPC_URL \
  --private-key $MULTISIG_PRIVATE_KEY
```

## Monitoring & Maintenance

### Event Monitoring

Set up monitoring for critical events:

```javascript
// Monitor contract events
const provider = new ethers.providers.JsonRpcProvider(RPC_URL);
const contract = new ethers.Contract(CONTRACT_ADDRESS, ABI, provider);

contract.on("AssetCreated", (asset, name, symbol, event) => {
  console.log(`New asset created: ${name} (${symbol}) at ${asset}`);
  // Send alert to monitoring system
});
```

### Health Checks

Regular health check script:

```bash
# Daily health check
#!/bin/bash
echo "Checking protocol health..."

# Check contract balances
cast balance $PROTOCOL_TREASURY --rpc-url $RPC_URL

# Check system status
cast call $SYSTEM_STATUS "isSystemHealthy()" --rpc-url $RPC_URL

# Check oracle prices
cast call $PRICE_ORACLE "getPrice(address)" $CMX_TOKEN --rpc-url $RPC_URL
```

## Upgrade Procedures

### Diamond Upgrade Process

1. **Prepare New Facets**

```bash
# Deploy new facet
forge script script/upgrades/DeployNewFacet.s.sol \
  --rpc-url $BASE_MAINNET_RPC_URL \
  --private-key $DEPLOYER_PRIVATE_KEY \
  --broadcast
```

2. **Propose Upgrade**

```bash
# Create upgrade proposal
cast send $GOVERNANCE_CONTRACT \
  "propose(address[],uint256[],bytes[],string)" \
  "[$DIAMOND_ADDRESS]" \
  "[0]" \
  "[$UPGRADE_CALLDATA]" \
  "Upgrade to add new facet functionality" \
  --rpc-url $BASE_MAINNET_RPC_URL \
  --private-key $PROPOSER_PRIVATE_KEY
```

3. **Execute Upgrade**

```bash
# After voting period and approval
cast send $GOVERNANCE_CONTRACT \
  "execute(uint256)" \
  $PROPOSAL_ID \
  --rpc-url $BASE_MAINNET_RPC_URL \
  --private-key $EXECUTOR_PRIVATE_KEY
```

## Troubleshooting

### Common Issues

**1. Gas Estimation Failures**

```bash
# Increase gas limit for complex transactions
cast send $CONTRACT \
  "function()" \
  --gas-limit 1000000 \
  --rpc-url $RPC_URL \
  --private-key $PRIVATE_KEY
```

**2. Verification Failures**

```bash
# Use specific Solidity version
forge verify-contract \
  --compiler-version 0.8.20+commit.a1b79de6 \
  $CONTRACT_ADDRESS \
  $CONTRACT_PATH \
  --etherscan-api-key $API_KEY
```

**3. Role Access Denied**

```bash
# Check role assignments
cast call $GLOBAL_ACCESS_MANAGER \
  "hasRole(bytes32,address)" \
  $ROLE_HASH \
  $USER_ADDRESS \
  --rpc-url $RPC_URL
```

### Recovery Procedures

**Emergency Pause**

```bash
# Pause protocol in emergency
cast send $EMERGENCY_PAUSE \
  "pause()" \
  --rpc-url $BASE_MAINNET_RPC_URL \
  --private-key $EMERGENCY_PRIVATE_KEY
```

**Role Recovery**

```bash
# Recover access through time-delayed admin
cast send $GLOBAL_ACCESS_MANAGER \
  "grantRole(bytes32,address,uint32)" \
  $ADMIN_ROLE \
  $RECOVERY_ADDRESS \
  0 \
  --rpc-url $BASE_MAINNET_RPC_URL \
  --private-key $TIMELOCK_PRIVATE_KEY
```

## Deployment Checklist

### Pre-Deployment

- [ ] All environment variables configured
- [ ] Sufficient gas funds in deployer accounts
- [ ] Contract compilation successful
- [ ] Unit tests passing
- [ ] Integration tests passing

### Deployment

- [ ] Core infrastructure deployed
- [ ] Access management configured
- [ ] Factory contracts deployed
- [ ] System contracts deployed
- [ ] All contracts verified

### Post-Deployment

- [ ] Access roles assigned
- [ ] Attestation schemas created
- [ ] Price oracles configured
- [ ] Subscription tiers set up
- [ ] Multi-signature ownership transferred
- [ ] Monitoring systems active
- [ ] Documentation updated

This comprehensive deployment guide ensures secure, verifiable, and maintainable deployment of the CMX Protocol across all supported networks.
