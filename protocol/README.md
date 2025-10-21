# Protocol Overview

CapSign Protocol is a modular smart contract framework for securities tokens and investment offerings built on the Diamond Pattern (EIP-2535).

## Core Features

1. **Wallets** - ERC-4337 smart accounts with biometric authentication
2. **Tokens** - ERC-7752 securities tokens with lot-based accounting
3. **Offerings** - Investment offerings with built-in compliance
4. **Documents** - Blockchain-based document signing
5. **Identity** - Decentralized attestations via EAS

## Architecture

### Diamond Pattern (EIP-2535)

All contracts use the Diamond Pattern for:

- **Modularity** - Features split into separate facets
- **Upgradeability** - Add/replace features without changing address
- **No size limit** - Bypass 24KB contract limit
- **Gas efficiency** - Singleton pattern reduces costs

Learn more: [Diamond Pattern](diamond-pattern.md)

### Deployed Contracts

- **Base Mainnet** (Chain ID: 8453)
- **Base Sepolia** (Chain ID: 84532)

See [Contract Addresses](/reference/contract-addresses.md) for deployed addresses.

## Core Components

### 1. Wallets

ERC-4337 smart contract wallets:

- Passkey authentication (WebAuthn/P256)
- EOA support (MetaMask, etc.)
- Account abstraction
- Context switching (multi-entity)

**Architecture:**
- `WalletFactory` - Deploys wallet diamonds via CREATE2
- `WalletCoreFacet` - Core ERC-4337 logic
- `WalletSignatureFacet` - ERC-1271 signature validation
- `WalletDocumentsFacet` - Document management
- `WalletIdentityFacet` - Attestation management

Learn more: [Wallet Architecture](wallets.md)

### 2. Tokens

ERC-7752 securities tokens with lot tracking:

- Standard ERC-20 compatibility
- Lot-based accounting (cost basis, acquisition date)
- Transfer restrictions (lockups, vesting, Rule 144, ROFR)
- Compliance modules

**Architecture:**
- `TokenFactory` - Deploys token diamonds
- `TokenERC20Facet` - ERC-20 implementation
- `TokenLotsFacet` - ERC-7752 lot management
- `TokenTransferFacet` - Transfer logic with compliance
- `TokenComplianceFacet` - Compliance condition checks
- `TokenAdminFacet` - Administrative functions

Learn more: [Token Architecture](tokens.md)

### 3. Offerings

Investment offerings with compliance:

- Hybrid escrow (investor protection + issuer control)
- Reg D (506b, 506c), Reg S, Reg A+ presets
- Automated token issuance
- Compliance modules (KYC, accreditation, investor limits)

**Architecture:**
- `OfferingFactory` - Deploys offering diamonds
- `OfferingCoreFacet` - Core offering logic (invest, countersign)
- `OfferingComplianceFacet` - Compliance checks
- `OfferingDocumentsFacet` - Document management
- Compliance modules (separate contracts for each rule)

Learn more: [Offering Architecture](offerings.md)

### 4. Documents

Blockchain-based document signing:

- Upload to IPFS/Arweave
- Cryptographic signatures
- EAS attestations
- Multi-party signing

**Architecture:**
- `WalletDocumentsFacet` - Per-wallet document storage
- Documents stored as EAS attestations
- Content stored off-chain (IPFS/Arweave)
- Only content hash on-chain

Learn more: [Document Architecture](documents.md)

### 5. Identity

Decentralized identity via EAS:

- KYC verification (Persona via Bridge.xyz)
- Attestations (accreditation, qualifications)
- Privacy-preserving credentials
- Smart contract verification

**Architecture:**
- Ethereum Attestation Service (EAS) for all attestations
- Schemas for each attestation type
- CapSign as attester for KYC
- Issuers as attesters for accreditation

Learn more: [Identity System](/identity/README.md)

## Key Patterns

### Factory Pattern

All deployments use factories:

```solidity
IWallet wallet = WalletFactory.createWallet(config);
IToken token = TokenFactory.createToken(config);
IOffering offering = OfferingFactory.createOffering(config);
```

Benefits:
- Deterministic addresses (CREATE2)
- Vanity addresses
- Consistent initialization
- Gas optimization

### Multi-Init Pattern

Diamonds initialize multiple facets in one transaction:

```solidity
MultiInit.init(
  [
    abi.encodeCall(WalletCore_init, (...)),
    abi.encodeCall(AccessControl_init, (...))
  ]
);
```

### Diamond Storage

All state uses diamond storage pattern:

```solidity
library MyStorage {
  bytes32 constant STORAGE_SLOT = keccak256("my.storage");
  
  struct Layout {
    mapping(address => uint256) balances;
  }
  
  function layout() internal pure returns (Layout storage l) {
    bytes32 slot = STORAGE_SLOT;
    assembly { l.slot := slot }
  }
}
```

## Development

### Prerequisites

- Foundry
- Node.js 18+
- pnpm

### Setup

```bash
# Clone
git clone https://github.com/capsign/protocol
cd protocol

# Install
pnpm install
forge install

# Build
forge build

# Test
forge test
```

### Testing

```bash
# Run all tests
forge test

# Run specific test
forge test --match-test testCreateToken

# Coverage
forge coverage

# Gas report
forge test --gas-report
```

### Deployment

```bash
# Set environment
export PRIVATE_KEY=...
export RPC_URL=...

# Deploy
forge script script/Deploy.s.sol --broadcast --verify
```

## Security

### Audits

Protocol audited by:
- [Pending] - Contact for audit report

### Bug Bounty

Report vulnerabilities: security@capsign.com

### Best Practices

- All facets use reentrancy guards
- Access control on all state-changing functions
- Pausability for emergency situations
- Events for all state changes

## Gas Optimization

### Diamond Pattern Benefits

- Facets deployed once, reused across diamonds
- ~90% gas savings on deployment
- No proxy overhead at runtime

### Other Optimizations

- Minimal storage reads/writes
- Batch operations where possible
- Efficient data structures
- Assembly for critical paths

## Guides

- [Diamond Pattern](diamond-pattern.md) - Understanding EIP-2535
- [Wallets](wallets.md) - Wallet architecture
- [Tokens](tokens.md) - Token implementation
- [Offerings](offerings.md) - Offering system
- [Documents](documents.md) - Document management

## Resources

- **Contracts:** [GitHub](https://github.com/capsign/protocol)
- **Deployments:** [Contract Addresses](/reference/contract-addresses.md)
- **Audits:** Coming soon
- **Whitepaper:** Coming soon

## Need Help?

- **Technical Support:** dev@capsign.com
- **Twitter:** [@CapSignInc](https://twitter.com/CapSignInc)
- **Discord:** [Coming Soon]
