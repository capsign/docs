# CMX Protocol

[![GitBook](https://img.shields.io/static/v1?message=Documented%20on%20GitBook&logo=gitbook&logoColor=ffffff&label=%20&labelColor=5c5c5c&color=3F89A1)](https://docs.capsign.com)

The Capital Markets (CMX) Protocol is a platform for creating and managing tokenized real-world assets (RWAs), enabling secure, compliant, and efficient digital representation of physical and financial assets.

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [For Developers](#for-developers)
  - [Getting Started](#getting-started)
  - [API Documentation](#api-documentation)
  - [Architecture](#architecture)
- [For Users](#for-users)
  - [Platform Access](#platform-access)
  - [User Guide](#user-guide)

## Overview

The Capital Markets Protocol provides a robust infrastructure for the tokenization of real-world assets including real estate, commodities, securities, and other traditional financial instruments. This protocol bridges traditional finance with blockchain technology, offering a suite of tools for RWA issuance, management, and transfer within a secure and compliant framework.

## Key Features

- **Real-World Asset Tokenization**: Convert traditional assets like real estate, securities, and commodities into digital tokens
- **Compliance Framework**: Built-in regulatory compliance tools designed specifically for RWA requirements
- **Security**: Enterprise-grade security protocols for protecting high-value asset representations
- **Interoperability**: Seamless integration with existing financial systems and asset management platforms
- **Scalability**: Designed to handle high transaction volumes across diverse asset classes
- **Transparency**: Auditable transaction history for all tokenized assets

## For Developers

> Capsign smart contracts are currently in private preview. To request early access, email developers@capsign.com.

### Getting Started

1. **Prerequisites**

   - Foundry development environment
   - Ethereum wallet and [Base Sepolia ETH](https://docs.base.org/chain/network-faucets)
   - Familiarity with Solidity and blockchain concepts

2. **Installation**

   ```bash
   git clone https://github.com/capsign/protocol.git capsign
   cd capsign

   # IMPORTANT: Install Foundry v1.1.0 (required due to parser regression in v1.2.3+)
   foundryup -i 1.1.0

   forge install
   ```

3. **Security Setup**

   For secure private key management, use interactive mode (recommended for most users):

   ```bash
   # Deploy with secure interactive private key input
   forge script script/deploy/DeployProtocol.s.sol \
     --rpc-url https://sepolia.base.org \
     --broadcast \
     --interactive
   ```

   Alternative approaches:

   ```bash
   # Using environment variables (ensure .env files are gitignored)
   export PRIVATE_KEY="0x..."
   forge script script/deploy/DeployProtocol.s.sol \
     --rpc-url https://sepolia.base.org \
     --broadcast \
     --private-key $PRIVATE_KEY

   # Using hardware wallet (most secure)
   forge script script/deploy/DeployProtocol.s.sol \
     --rpc-url https://sepolia.base.org \
     --broadcast \
     --ledger
   ```

   See our [Security Guidelines](SECURITY.md) for comprehensive security best practices and additional options.

4. **Post-Deployment Verification**

   After deploying contracts, verify the deployment was successful using the verification script:

   ```bash
   # Verify deployment on Base Sepolia testnet
   forge script script/verify/PostDeploymentVerification.s.sol \
     --rpc-url https://sepolia.base.org \
     --chain-id 84532

   # Verify deployment on Base Mainnet
   forge script script/verify/PostDeploymentVerification.s.sol \
     --rpc-url https://mainnet.base.org \
     --chain-id 8453
   ```

   This script will verify that all protocol components are properly deployed and configured, displaying contract addresses and their status.

5. **Integration**

   We recommend using our Node.js SDK for interacting with CapSign contracts:

   ```bash
   npm install @capsign/sdk
   ```

   ```javascript
   // Example SDK usage
   const CapSign = require("@capsign/sdk");

   // Initialize connection to CapSign contracts
   const capsign = new CapSign.Client({
     network: "base-sepolia",
     privateKey: process.env.PRIVATE_KEY, // Set securely in your environment
   });

   // Interact with tokenized assets
   const myAssets = await capsign.assets.getAll();
   ```

### API Documentation

Comprehensive API documentation is available at [docs.capsign.com/api-reference](https://docs.capsign.com/api-reference).

> **Note:** CapSign currently only supports the Base Sepolia testnet. Mainnet support coming soon.

### Architecture

CapSign is built on a trustless, self-custodial architecture:

- **100% On-Chain Operations**: All asset management and transactions occur on-chain
- **Self-Custodial Design**: Users maintain complete control of their assets
- **Decentralized ID Attestation**: The only component with an off-chain element, providing secure identity verification
- **Smart Contract Layer**: Secure and audited contracts for asset tokenization and management

## For Users

### Platform Access

Visit [app.capsign.com](https://app.capsign.com) to access the user interface.

### User Guide

Our [User Guide](https://docs.capsign.com/guides) provides detailed instructions for:

- Tokenizing real-world assets
- Managing asset lifecycle events (dividends, voting, etc.)
- Setting compliance parameters for regulated assets
- Executing compliant transfers and transactions
- Reporting and analytics for portfolio management

## Regulatory Considerations

CapSign is designed with regulatory compliance as a foundational principle for real-world asset tokenization:

- **KYC/AML Integration**: Built-in support for Know Your Customer and Anti-Money Laundering protocols required for regulated assets
- **Jurisdictional Flexibility**: Configurable rulesets to adapt to different regulatory environments across global markets
- **Audit Trails**: Comprehensive record-keeping for regulatory reporting and asset provenance
- **Compliance Documentation**: [Compliance framework documentation](https://docs.capsign.com/compliance) for regulators

## Licensing

BUSL-1.1, see [LICENSE](./LICENSE.md) for more details.

Refer to file headers and directory-level `README.md` files for more details.

## Support and Community

- **Documentation**: [docs.capsign.com](https://docs.capsign.com)
- **Community Forum**: [forum.capsign.com](https://forum.capsign.com)
- **Technical Support**: [support@capsign.com](mailto:support@capsign.com)
- **Discord**: [Join our Discord](https://discord.gg/gSmnZ9wmNv)

## Contributing

We welcome contributions from the community. Please see our [Contributing Guidelines](CONTRIBUTING.md) for more information.

## Security

If you discover a security vulnerability, please follow our [Security Policy](SECURITY.md) for responsible disclosure.
