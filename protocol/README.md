# Protocol Overview

CapSign Protocol is a comprehensive smart contract framework for private securities and capital markets built on the Diamond Pattern (EIP-2535).

## Architecture

The protocol uses a **diamond-based architecture** for maximum upgradability and modularity:

- **Modular Facets** - Functionality split into separate contracts
- **Upgradeable** - Add/remove/replace features without redeploying
- **Single Address** - All functionality accessible via one diamond address
- **No Size Limit** - Bypass 24KB contract size limit

Learn more: [Architecture](/protocol/architecture.md)

## Core Components

### Smart Accounts (ERC-4337)

Non-custodial wallets with programmable logic:

- Passkey authentication (WebAuthn/P256)
- EOA wallet support (MetaMask, etc.)
- Gasless transactions via paymasters
- Social recovery mechanisms

Learn more: [Smart Accounts](/protocol/smart-accounts.md)

### Attestations (EAS Integration)

On-chain credentials for identity and compliance:

- KYC/AML verification
- Accredited investor status
- Professional credentials
- Privacy-preserving proofs

Learn more: [Attestations](/protocol/attestations.md)

### Document Registry

Blockchain-based document signing:

- Cryptographic signatures
- Immutable records
- Multi-party signing
- Verification tools

Learn more: [Documents](/protocol/documents.md)

### Access Control

Enterprise-grade permission management:

- Role-based access control (RBAC)
- Multi-signature requirements
- Time-delayed actions
- Emergency pause mechanisms

Learn more: [Access Control](/protocol/access-control.md)

## Smart Contract Modules

### Wallets
- `WalletFactory` - Deploys diamond wallets
- `WalletCoreFacet` - Core wallet functionality
- `WalletSignatureFacet` - Signature validation

### Attestations
- `AttestationRegistry` - Manages attestations
- `AttestationCoreFacet` - Core attestation logic
- `AttestationQueryFacet` - Query attestations

### Documents
- `DocumentRegistry` - Document management
- `DocumentSigningFacet` - Signature logic
- `DocumentCoreFacet` - Core document operations

### Access Management
- `GlobalAccessManager` - System-wide permissions
- `IdentityAccessManager` - Per-account permissions
- `AccessControlFacet` - Access checks

## Development

### Prerequisites

- Foundry development environment
- Node.js 18+
- pnpm package manager
- Basic Solidity knowledge

### Quick Start

```bash
# Clone repository
git clone https://github.com/capsign/protocol.git
cd protocol

# Install dependencies
forge install
pnpm install

# Run tests
forge test

# Deploy locally
forge script script/deploy/DeployLocal.s.sol --broadcast
```

### Testing

```bash
# Run all tests
forge test

# Run specific test
forge test --match-test testWalletDeployment

# Run with gas reports
forge test --gas-report

# Run with coverage
forge coverage
```

Learn more: [Testing](/protocol/testing.md)

### Deployment

Deploy protocol contracts to testnet or mainnet:

```bash
# Deploy to Base Sepolia
forge script script/deploy/DeployBaseSepolia.s.sol --broadcast --verify

# Deploy to Base Mainnet
forge script script/deploy/DeployBase.s.sol --broadcast --verify
```

Learn more: [Deployment](/protocol/deployment.md)

## Security

### Audit Status

- Internal security reviews ongoing
- External audits planned before mainnet
- Bug bounty program coming soon

### Best Practices

- All privileged functions use access control
- Emergency pause mechanisms
- Time delays for critical operations
- Comprehensive event logging

## Smart Contract Standards

- **EIP-2535**: Diamond Pattern
- **ERC-4337**: Account Abstraction
- **EIP-712**: Typed Data Signing
- **EIP-1271**: Contract Signature Validation
- **ERC-7752**: Lot-Based Tokens (CapSign extension)

## Documentation Structure

- **[Architecture](/protocol/architecture.md)** - System design and diamond pattern
- **[Smart Accounts](/protocol/smart-accounts.md)** - ERC-4337 wallets
- **[Attestations](/protocol/attestations.md)** - Identity credentials
- **[Documents](/protocol/documents.md)** - Document management
- **[Access Control](/protocol/access-control.md)** - Permission system
- **[Deployment](/protocol/deployment.md)** - Deploy contracts
- **[Testing](/protocol/testing.md)** - Test your contracts
- **[API Reference](/api-reference/README.md)** - Complete contract API

## Examples

### Deploy a Wallet

```solidity
// Get factory
IWalletFactory factory = IWalletFactory(walletFactoryAddress);

// Owner credentials (P256 public key for passkey)
P256.PublicKey memory owner = P256.PublicKey({
    x: publicKeyX,
    y: publicKeyY
});

// Deploy wallet
address wallet = factory.createWallet(owner);
```

### Issue an Attestation

```solidity
// Get attestation registry
IAttestationRegistry registry = IAttestationRegistry(registryAddress);

// Attestation data
AttestationRequest memory request = AttestationRequest({
    schema: KYC_SCHEMA,
    recipient: userAddress,
    expirationTime: block.timestamp + 365 days,
    revocable: true,
    data: abi.encode(verificationLevel)
});

// Issue attestation
bytes32 attestationUID = registry.attest(request);
```

### Sign a Document

```solidity
// Get document registry
IDocumentRegistry registry = IDocumentRegistry(registryAddress);

// Document hash
bytes32 documentHash = keccak256(documentContent);

// Sign document
registry.signDocument(documentHash, signature);
```

## Resources

- **GitHub**: [github.com/capsign/protocol](https://github.com/capsign/protocol)
- **Discord**: [Join community](https://discord.gg/gSmnZ9wmNv)
- **Twitter**: [@CapSignInc](https://twitter.com/CapSignInc)
- **Email**: dev@capsign.com

## License

Business Source License 1.1 (BUSL-1.1) - See [LICENSE](/LICENSE) for details.

---

**Next**: [Architecture](/protocol/architecture.md) →
