# CapSign Documentation

## What is CapSign?

CapSign is a **private investing platform** that combines cutting-edge blockchain technology with user-friendly design to provide:

- **Smart Accounts** - Non-custodial wallets with biometric authentication (Face ID/Touch ID)
- **Multi-Entity Workflows** - Access multiple organizations from a single account (e.g., your personal account, corporate entities, investment funds, SPVs)
- **Digital Identity** - Decentralized identity verification using Ethereum Attestation Service
- **Document Management** - Secure, verifiable document signing and storage
- **Compliance Framework** - Built-in KYC/AML and regulatory compliance tools for private markets

## Quick Start

### For Users

Get started with CapSign in minutes:

1. **[Create Your Account](/getting-started/quickstart.md)** - Set up your smart wallet with biometric authentication in 30 seconds
2. **[Explore Key Concepts](/getting-started/key-concepts.md)** - Understand smart accounts, attestations, and multi-entity workflows
3. **[Complete Identity Verification](/guides/identity-verification.md)** - Verify your identity when you're ready to invest

**New to CapSign?** Check out our **[User Guides](/guides/)** for step-by-step instructions.

### Multi-Entity Support

CapSign supports complex organizational structures through context switching:

- **Individual investors** - Manage your personal investments
- **Corporate entities** - Access your company's investment accounts
- **Fund managers** - Switch between multiple SPVs and fund entities
- **Multi-role users** - Seamlessly transition between roles (e.g., employee of Corp A, LP in Fund B, GP of Fund C)

Switch contexts instantly from the account menu - no need to log in and out.

### For Developers

Build on the CapSign platform:

1. **[Protocol Overview](/protocol/README.md)** - Understand the smart contract architecture
2. **[Interface Documentation](/interface/README.md)** - Integrate CapSign into your frontend
3. **[API Reference](/api-reference/README.md)** - Explore the complete contract API

**Building a dApp?** Start with our **[Interface Documentation](/interface/)** to integrate authentication, attestations, and wallet features.

## Documentation Structure

### Getting Started
- **[Quickstart](/getting-started/quickstart.md)** - Create your account and get started
- **[Key Concepts](/getting-started/key-concepts.md)** - Learn about smart accounts, attestations, and passkeys
- **[FAQ](/getting-started/faq.md)** - Common questions and answers

### User Guides
- **[Creating Your Account](/guides/creating-your-account.md)** - Account creation walkthrough
- **[Identity Verification](/guides/identity-verification.md)** - Complete KYC verification
- **[Managing Your Wallet](/guides/managing-your-wallet.md)** - Wallet operations and management
- **[Signing Documents](/guides/signing-documents.md)** - Document signing workflow
- **[Viewing Attestations](/guides/viewing-attestations.md)** - Understand your identity attestations
- **[Security Best Practices](/guides/security-best-practices.md)** - Keep your account secure

### Protocol Documentation
- **[Protocol Overview](/protocol/README.md)** - Smart contract architecture
- **[Architecture](/protocol/architecture.md)** - Diamond pattern and system design
- **[Smart Accounts](/protocol/smart-accounts.md)** - Wallet contracts and ERC-4337
- **[Attestations](/protocol/attestations.md)** - Identity verification system
- **[Documents](/protocol/documents.md)** - Document registry and signing
- **[Access Control](/protocol/access-control.md)** - Permission management
- **[Deployment](/protocol/deployment.md)** - Deploy contracts
- **[Testing](/protocol/testing.md)** - Test your integrations

### Interface Documentation
- **[Interface Overview](/interface/README.md)** - Frontend integration guide
- **[Authentication](/interface/authentication.md)** - Account creation and login
- **[Passkeys](/interface/passkeys.md)** - Passkey implementation
- **[Attestations](/interface/attestations.md)** - Attestation UI/UX
- **[Direct Signing](/interface/direct-signing.md)** - EOA wallet integration
- **[Context Management](/interface/context-management.md)** - Multi-account switching
- **[Components](/interface/components.md)** - UI component library
- **[API Integration](/interface/api-integration.md)** - Backend API usage

### Advanced Topics
- **[ERC-7752 Lot Tokens](/advanced/erc-7752.md)** - Lot-based token standard
- **[Diamond Upgrades](/advanced/diamond-upgrades.md)** - Upgrading diamond contracts
- **[Gas Optimization](/advanced/gas-optimization.md)** - Performance optimization
- **[Troubleshooting](/advanced/troubleshooting.md)** - Common issues and solutions

### Reference
- **[API Reference](/api-reference/README.md)** - Complete contract documentation
- **[Glossary](/reference/glossary.md)** - Technical terms and definitions
- **[Contract Addresses](/reference/contract-addresses.md)** - Deployed contract addresses
- **[Supported Networks](/reference/supported-networks.md)** - Network information

## Key Features

### Instant Account Creation

Create your smart wallet in seconds using Face ID or Touch ID - no seed phrases, no complexity. Identity verification can be completed later when you're ready to invest.

### Multi-Entity Management

Access all your roles from a single account:

- **As an investor**: Manage your personal portfolio
- **As a corporate officer**: Access company investment accounts
- **As a fund manager**: Manage multiple SPVs and fund entities
- **As a service provider**: Work with multiple client entities

Switch between entities instantly from the account context menu.

### Private Market Compliance

Built-in compliance tools for private securities:

- KYC/AML verification workflows
- Accredited investor attestations
- Document signing and management
- Regulatory reporting

## Technology Stack

CapSign is built with:

- **Smart Contracts** - Solidity with Diamond Pattern (EIP-2535)
- **Smart Accounts** - ERC-4337 account abstraction with Coinbase Smart Wallet
- **Identity** - Ethereum Attestation Service (EAS)
- **Frontend** - Next.js, TypeScript, Wagmi, Viem
- **Blockchain** - Base (primary), Ethereum (cross-chain)

## Community & Support

- **Discord** - [Join our community](https://discord.gg/gSmnZ9wmNv)
- **GitHub** - [View source code](https://github.com/capsign)
- **Email** - [support@capsign.com](mailto:support@capsign.com)
- **Twitter** - [@capsign](https://twitter.com/CapSignInc)

## Contributing

We welcome contributions to our documentation! Found an error or want to improve something?

1. Fork the [documentation repository](https://github.com/capsign/docs)
2. Make your changes
3. Submit a pull request with a clear description

See our **[Contributing Guide](/CONTRIBUTING.md)** for more details.

## License

The CapSign Protocol is licensed under the Business Source License 1.1 (BUSL-1.1). See [LICENSE](/LICENSE) for details.

---

**Ready to get started?** Create your account at [app.capsign.com](https://app.capsign.com) or explore our **[Quickstart Guide](/getting-started/quickstart.md)**.
