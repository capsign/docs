# CapSign Documentation

Welcome to CapSign - a **private investing platform** that makes it easy to create, issue, and invest in securities tokens.

## What is CapSign?

CapSign combines blockchain technology with compliance tools to provide:

- **30-second account creation** - Create a smart wallet instantly with Face ID or Touch ID (no email required)
- **Multi-entity support** - Manage personal, corporate, and fund accounts from a single login
- **Securities tokens** - Issue compliant digital securities with built-in lot tracking (ERC-7752)
- **Investment offerings** - Create private offerings with Reg D, Reg S, or Reg A+ compliance
- **Document signing** - Sign legal documents with blockchain-based signatures
- **Identity verification** - Verify identity through Persona (via Bridge.xyz) when ready to invest

## Quick Start

### For Users

1. **[Create Your Account](/getting-started/quickstart.md)** - Set up your wallet in 30 seconds
2. **Explore the Platform** - Browse offerings and familiarize yourself with features
3. **[Complete Identity Verification](/identity/verification.md)** - When ready to invest

### For Issuers

1. **[Create Your Account](/getting-started/quickstart.md)** - Set up your entity account
2. **[Create a Token](/tokens/creating-a-token.md)** - Issue your security token
3. **[Create an Offering](/offerings/creating-an-offering.md)** - Launch your investment offering

### For Developers

1. **[Protocol Overview](/protocol/README.md)** - Understand the smart contract architecture
2. **[Diamond Pattern](/protocol/diamond-pattern.md)** - Learn about our modular contract system
3. **[Contract Addresses](/reference/contract-addresses.md)** - Find deployed contract addresses

## Core Features

### 1. Wallets

Smart contract wallets (ERC-4337) with biometric authentication:

- **Instant setup** - No seed phrases, no complex onboarding
- **Biometric security** - Face ID / Touch ID authentication
- **Multi-entity support** - Switch between personal, corporate, and fund contexts
- **Account abstraction** - Gasless transactions and sponsored gas

**Learn more:** [Wallet Documentation](/wallets/README.md)

### 2. Tokens

ERC-7752 securities tokens with lot-based accounting:

- **Create tokens** - Issue shares, units, or other securities
- **Lot tracking** - Automatic cost basis and acquisition date tracking
- **Transfer restrictions** - Lockups, vesting, Rule 144, ROFR
- **Compliance** - Built-in compliance modules

**Learn more:** [Token Documentation](/tokens/README.md)

### 3. Offerings

Investment offerings with built-in compliance:

- **Create offerings** - Set price, minimum investment, and maximum raise
- **Compliance presets** - Reg D (506b, 506c), Reg S, Reg A+
- **Investor management** - Track investments and issue tokens
- **Hybrid escrow** - Protect both issuers and investors

**Learn more:** [Offering Documentation](/offerings/README.md)

### 4. Documents

Blockchain-based document management:

- **Upload documents** - Store documents on IPFS or Arweave
- **Sign documents** - Cryptographic signatures with biometric auth
- **Verify signatures** - Anyone can verify document authenticity
- **Attestations** - Documents stored as EAS attestations

**Learn more:** [Document Documentation](/documents/README.md)

### 5. Identity

Decentralized identity and attestations:

- **KYC verification** - Identity verification through Persona (via Bridge.xyz)
- **Attestations** - Privacy-preserving credentials on Ethereum Attestation Service
- **Issuer attestations** - Issuers can attest to accreditation and qualifications
- **Portable credentials** - Use your attestations across multiple offerings

**Learn more:** [Identity Documentation](/identity/README.md)

## Multi-Entity Workflows

CapSign supports complex organizational structures through **context switching**:

- **Individual investors** - Personal investment accounts
- **Corporate entities** - Company investment accounts
- **Fund managers** - Manage multiple funds and SPVs
- **Multi-role users** - Switch between roles (GP, LP, employee, etc.)

**Example**: You're a fund manager with 3 SPVs and also an LP in another fund:
- Context 1: Personal account
- Context 2: SPV Alpha (GP)
- Context 3: SPV Beta (GP)
- Context 4: SPV Gamma (GP)
- Context 5: XYZ Fund (LP)

Switch contexts instantly from the account menu - no logging in and out.

**Learn more:** [Multi-Entity Accounts](/wallets/multi-entity-accounts.md)

## Technology

### For Users

- **Smart Wallets** - ERC-4337 account abstraction
- **Passkeys** - WebAuthn biometric authentication
- **Gasless Transactions** - Platform sponsors gas for certain operations
- **Base Network** - Low-cost, fast Ethereum L2

### For Developers

- **Diamond Pattern** - Modular, upgradeable contracts (EIP-2535)
- **ERC-7752** - Lot-based token standard for securities
- **ERC-4337** - Account abstraction standard
- **Ethereum Attestation Service** - Decentralized identity credentials
- **Viem + Wagmi** - Modern TypeScript Ethereum libraries

**Learn more:** [Protocol Documentation](/protocol/README.md)

## Key Concepts

- **Smart Wallet** - A smart contract wallet controlled by biometric authentication (not a traditional EOA)
- **Attestation** - A cryptographic credential that proves something about you (identity, accreditation, etc.)
- **Lot** - A group of tokens with the same acquisition date and cost basis (for tax tracking)
- **Compliance Module** - A smart contract that enforces transfer restrictions or investment requirements
- **Diamond** - A modular smart contract architecture that allows for upgrades and extensions

**Learn more:** [Glossary](/reference/glossary.md)

## Resources

- **[Quickstart Guide](/getting-started/quickstart.md)** - Get started in 5 minutes
- **[FAQ](/getting-started/faq.md)** - Frequently asked questions
- **[Contract Addresses](/reference/contract-addresses.md)** - Deployed contract addresses
- **[Supported Networks](/reference/supported-networks.md)** - Base Mainnet and Base Sepolia

## Support

- **Twitter:** [@CapSignInc](https://twitter.com/CapSignInc)
- **GitHub:** [github.com/capsign](https://github.com/capsign)
- **Email:** support@capsign.com

## Legal

CapSign provides technology tools but does not provide legal, financial, or tax advice. You are responsible for ensuring compliance with all applicable laws and regulations. Consult with qualified legal counsel before issuing securities.
