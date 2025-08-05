# CMX Protocol

The CMX Protocol is a comprehensive capital markets blockchain infrastructure providing enterprise-grade smart contracts for tokenized securities, asset management, trading, and regulatory compliance. Built with regulatory compliance and institutional-grade security at its core.

## Architecture Overview

The CMX Protocol uses a **diamond-based architecture** (EIP-2535) for maximum upgradability and modularity while maintaining security and regulatory compliance.

### Core Components

- **🏛️ Diamond Architecture** - Modular, upgradeable smart contracts
- **🔐 Access Management** - Enterprise-grade permission system
- **📋 Attestation System** - Identity verification and compliance
- **💎 Asset Tokenization** - Securities and RWA tokenization
- **📊 Trading Markets** - Multiple trading venue types
- **⚖️ Governance** - Corporate governance and voting mechanisms
- **📚 Document Management** - Secure document storage and verification
- **💰 Payment Systems** - Gas abstraction and billing
- **📖 Accounting Ledgers** - Double-entry bookkeeping
- **🏦 Fund Management** - Investment fund operations

## Key Features

### Regulatory Compliance First

- **Built-in KYC/AML** - Integrated identity verification
- **Securities Law Compliance** - Rule 506(b), 506(c), Rule 701 support
- **Multi-jurisdictional** - Configurable for different regulatory environments
- **Audit Trails** - Complete transaction history for regulatory reporting

### Enterprise Security

- **Diamond Architecture** - Secure upgradeable contracts
- **Role-based Access** - Granular permission system
- **Multi-signature** - Enterprise governance controls
- **Security-First Design** - Built with security best practices

### Capital Markets Focus

- **Securities Tokenization** - Equity, debt, and hybrid instruments
- **Investment Funds** - Mutual funds, hedge funds, ETFs
- **Trading Infrastructure** - Order books, auctions, OTC markets
- **Settlement Systems** - Automated clearing and settlement


**For detailed information about our lot-based token implementation, see [ERC-7752: Lot-Based Tokens](erc-7752.md).**

## Smart Contract Modules

### Core Infrastructure

- **[Diamond Factories](contracts.md#diamond-factories)** - Factory contracts for creating diamond-based assets
- **[Access Management](contracts.md#access-management)** - Unified access control system
- **[Registries](contracts.md#registries)** - Central contract and facet registration

### Compliance & Identity

- **[Attestation System](contracts.md#attestations)** - Decentralized identity verification
- **[KYC/AML Framework](contracts.md#kyc-aml)** - Regulatory compliance tools
- **[Document Registry](contracts.md#documents)** - Secure document management

### Asset Management

- **[Tokenization Framework](contracts.md#tokenization)** - Real-world asset tokenization
- **[Share Classes](contracts.md#share-classes)** - Equity instrument management
- **[Fund Management](contracts.md#funds)** - Investment fund operations

### Trading & Markets

- **[Order Book Markets](contracts.md#order-books)** - Traditional exchange-style trading
- **[Auction Markets](contracts.md#auctions)** - Price discovery mechanisms
- **[OTC Markets](contracts.md#otc)** - Over-the-counter trading
- **[Settlement](contracts.md#settlement)** - Automated trade settlement

### Governance & Operations

- **[Corporate Governance](contracts.md#governance)** - Board and shareholder decision making
- **[Accounting Ledgers](contracts.md#ledgers)** - Double-entry bookkeeping
- **[Subscription Billing](contracts.md#billing)** - Payment and subscription management

## Getting Started

### For Developers

1. **Prerequisites**

   - Foundry development environment
   - Node.js 18+ and npm/yarn
   - Basic understanding of Solidity and Diamond patterns

2. **Installation**

   ```bash
   git clone https://github.com/capsign/protocol.git
   cd protocol
   forge install
   ```

3. **Development Setup**

   ```bash
   # Copy environment template
   cp .env.example .env

   # Install dependencies
   npm install

   # Run tests
   forge test
   ```

### For Integrators

Use the CMX Protocol SDK for easy integration:

```bash
npm install @capsign/sdk
```

```javascript
import { CMXClient, Network } from "@capsign/sdk";

const client = new CMXClient({
  network: Network.BASE_SEPOLIA,
  privateKey: process.env.PRIVATE_KEY,
});

// Create a tokenized asset
const asset = await client.assets.create({
  name: "Example Corp Class A",
  symbol: "EXMP-A",
  totalShares: 1000000,
});
```

## Security Considerations

### Security Status

- **Active Development** - Contracts are in active development and testing
- **Internal Review** - Ongoing internal security review and testing
- **Future Audits** - Professional security audits planned before mainnet deployment

### Best Practices

- All privileged functions use multi-signature requirements
- Role-based access control with time-delayed administrative actions
- Emergency pause mechanisms for critical functions
- Comprehensive event logging for transparency

## Deployment Networks

### Testnets

- **Base Sepolia** - Primary testnet deployment
- **Ethereum Sepolia** - Cross-chain testing

### Mainnets

- **Base Mainnet** - Primary production deployment
- **Ethereum Mainnet** - Cross-chain bridge deployment

## Documentation Structure

- **[Smart Contracts](contracts.md)** - Detailed contract documentation
- **[Architecture](architecture.md)** - System design and patterns
- **[Deployment Guide](deployment.md)** - How to deploy the protocol
- **[Integration Guide](integration.md)** - SDK and API usage
- **[Governance](governance.md)** - Protocol governance mechanisms
- **[Security](security.md)** - Security model and considerations

## Developer Resources

- **[API Reference](api.md)** - Complete API documentation
- **[SDK Documentation](sdk.md)** - TypeScript SDK guide
- **[Testing Guide](testing.md)** - How to test integrations
- **[Examples](examples.md)** - Code examples and tutorials

## Compliance Documentation

- **[Regulatory Framework](compliance.md)** - Regulatory compliance approach
- **[KYC/AML Implementation](kyc-aml.md)** - Identity verification system
- **[Securities Law](securities.md)** - Securities regulation compliance

## License

Business Source License 1.1 (BUSL-1.1) - See [LICENSE](../LICENSE.md) for details.

## Support

- **Documentation**: [docs.capsign.com](https://docs.capsign.com)
- **Technical Support**: [support@capsign.com](mailto:support@capsign.com)
- **Discord**: [Join our Discord](https://discord.gg/capsign)
- **GitHub Issues**: [Report issues](https://github.com/capsign/protocol/issues)

The CMX Protocol is a comprehensive capital markets blockchain infrastructure providing enterprise-grade smart contracts for tokenized securities, asset management, trading, and regulatory compliance. Built with regulatory compliance and institutional-grade security at its core.

## Architecture Overview

The CMX Protocol uses a **diamond-based architecture** (EIP-2535) for maximum upgradability and modularity while maintaining security and regulatory compliance.

### Core Components

- **🏛️ Diamond Architecture** - Modular, upgradeable smart contracts
- **🔐 Access Management** - Enterprise-grade permission system
- **📋 Attestation System** - Identity verification and compliance
- **💎 Asset Tokenization** - Securities and RWA tokenization
- **📊 Trading Markets** - Multiple trading venue types
- **⚖️ Governance** - Corporate governance and voting mechanisms
- **📚 Document Management** - Secure document storage and verification
- **💰 Payment Systems** - Gas abstraction and billing
- **📖 Accounting Ledgers** - Double-entry bookkeeping
- **🏦 Fund Management** - Investment fund operations

## Key Features

### Regulatory Compliance First

- **Built-in KYC/AML** - Integrated identity verification
- **Securities Law Compliance** - Rule 506(b), 506(c), Rule 701 support
- **Multi-jurisdictional** - Configurable for different regulatory environments
- **Audit Trails** - Complete transaction history for regulatory reporting

### Enterprise Security

- **Diamond Architecture** - Secure upgradeable contracts
- **Role-based Access** - Granular permission system
- **Multi-signature** - Enterprise governance controls
- **Security-First Design** - Built with security best practices

### Capital Markets Focus

- **Securities Tokenization** - Equity, debt, and hybrid instruments
- **Investment Funds** - Mutual funds, hedge funds, ETFs
- **Trading Infrastructure** - Order books, auctions, OTC markets
- **Settlement Systems** - Automated clearing and settlement

## Smart Contract Modules

### Core Infrastructure

- **[Diamond Factories](contracts.md#diamond-factories)** - Factory contracts for creating diamond-based assets
- **[Access Management](contracts.md#access-management)** - Unified access control system
- **[Registries](contracts.md#registries)** - Central contract and facet registration

### Compliance & Identity

- **[Attestation System](contracts.md#attestations)** - Decentralized identity verification
- **[KYC/AML Framework](contracts.md#kyc-aml)** - Regulatory compliance tools
- **[Document Registry](contracts.md#documents)** - Secure document management

### Asset Management

- **[Tokenization Framework](contracts.md#tokenization)** - Real-world asset tokenization
- **[Share Classes](contracts.md#share-classes)** - Equity instrument management
- **[Fund Management](contracts.md#funds)** - Investment fund operations

### Trading & Markets

- **[Order Book Markets](contracts.md#order-books)** - Traditional exchange-style trading
- **[Auction Markets](contracts.md#auctions)** - Price discovery mechanisms
- **[OTC Markets](contracts.md#otc)** - Over-the-counter trading
- **[Settlement](contracts.md#settlement)** - Automated trade settlement

### Governance & Operations

- **[Corporate Governance](contracts.md#governance)** - Board and shareholder decision making
- **[Accounting Ledgers](contracts.md#ledgers)** - Double-entry bookkeeping
- **[Subscription Billing](contracts.md#billing)** - Payment and subscription management

## Getting Started

### For Developers

1. **Prerequisites**

   - Foundry development environment
   - Node.js 18+ and npm/yarn
   - Basic understanding of Solidity and Diamond patterns

2. **Installation**

   ```bash
   git clone https://github.com/capsign/protocol.git
   cd protocol
   forge install
   ```

3. **Development Setup**

   ```bash
   # Copy environment template
   cp .env.example .env

   # Install dependencies
   npm install

   # Run tests
   forge test
   ```

### For Integrators

Use the CMX Protocol SDK for easy integration:

```bash
npm install @capsign/sdk
```

```javascript
import { CMXClient, Network } from "@capsign/sdk";

const client = new CMXClient({
  network: Network.BASE_SEPOLIA,
  privateKey: process.env.PRIVATE_KEY,
});

// Create a tokenized asset
const asset = await client.assets.create({
  name: "Example Corp Class A",
  symbol: "EXMP-A",
  totalShares: 1000000,
});
```

## Security Considerations

### Security Status

- **Active Development** - Contracts are in active development and testing
- **Internal Review** - Ongoing internal security review and testing
- **Future Audits** - Professional security audits planned before mainnet deployment

### Best Practices

- All privileged functions use multi-signature requirements
- Role-based access control with time-delayed administrative actions
- Emergency pause mechanisms for critical functions
- Comprehensive event logging for transparency

## Deployment Networks

### Testnets

- **Base Sepolia** - Primary testnet deployment
- **Ethereum Sepolia** - Cross-chain testing

### Mainnets

- **Base Mainnet** - Primary production deployment
- **Ethereum Mainnet** - Cross-chain bridge deployment

## Documentation Structure

- **[Smart Contracts](contracts.md)** - Detailed contract documentation
- **[Architecture](architecture.md)** - System design and patterns
- **[Deployment Guide](deployment.md)** - How to deploy the protocol
- **[Integration Guide](integration.md)** - SDK and API usage
- **[Governance](governance.md)** - Protocol governance mechanisms
- **[Security](security.md)** - Security model and considerations

## Developer Resources

- **[API Reference](api.md)** - Complete API documentation
- **[SDK Documentation](sdk.md)** - TypeScript SDK guide
- **[Testing Guide](testing.md)** - How to test integrations
- **[Examples](examples.md)** - Code examples and tutorials

## Compliance Documentation

- **[Regulatory Framework](compliance.md)** - Regulatory compliance approach
- **[KYC/AML Implementation](kyc-aml.md)** - Identity verification system
- **[Securities Law](securities.md)** - Securities regulation compliance

## License

Business Source License 1.1 (BUSL-1.1) - See [LICENSE](../LICENSE.md) for details.

## Support

- **Documentation**: [docs.capsign.com](https://docs.capsign.com)
- **Technical Support**: [support@capsign.com](mailto:support@capsign.com)
- **Discord**: [Join our Discord](https://discord.gg/capsign)
- **GitHub Issues**: [Report issues](https://github.com/capsign/protocol/issues)
