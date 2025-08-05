# CMX Ecosystem Documentation

Welcome to CapSign's comprehensive documentation hub. This resource covers the complete **CMX ecosystem**: the Capital Markets Protocol smart contracts, CMX Network deployment, infrastructure tools, and developer resources.

**New to CapSign?** Start with our [Interactive Demos](/demos/README.md) to experience the platform without creating an account.

## About CapSign & CMX

**CapSign Inc.** is the company that built and maintains the CMX ecosystem. Here's how the components work together:

### **CapSign Inc.**

- **The Company**: Financial technology company focused on capital markets infrastructure
- **Role**: Develops, maintains, and provides enterprise services for the CMX ecosystem
- **Services**: Managed infrastructure, enterprise support, compliance consulting

### **Capital Markets Protocol**

- **What it is**: Multi-chain smart contract framework for tokenizing and trading capital markets assets
- **Built for**: Regulatory compliance, institutional-grade security, and capital markets workflows
- **Technology**: Diamond Pattern (EIP-2535) deployable on Ethereum, CMX Network, Base, and other EVM chains

### **CMX Network**

- **What it is**: Optimism-based Layer 2 blockchain optimized for capital markets
- **Purpose**: Fast, low-cost transactions while maintaining Ethereum security
- **Features**: Built-in compliance, institutional custody support, regulatory reporting

### **CMX Token**

- **What it is**: Native utility token of the CMX Network
- **Uses**: Network gas fees, service payments (20% discount), governance participation
- **Advantage**: Predictable pricing with 7-day rate locks for enterprise budgeting

**In Summary**: CapSign Inc. is the company behind the CMX ecosystem, which includes the Capital Markets Protocol (smart contracts), CMX Network (blockchain), and CMX Token (utility token).

## Documentation Structure

### Getting Started

- **[Choose Your Path](/getting-started/README.md)** - Managed service vs self-hosted decision guide
- **[Managed Service](/getting-started/managed.md)** - Let CapSign handle infrastructure (recommended)
- **[Self-Hosted](/getting-started/self-hosted.md)** - Deploy and manage infrastructure yourself

### Interactive Demos

- **[Demo Overview](/demos/README.md)** - Hands-on experience without an account
- **[Create Account](/demos/create-account.md)** - Smart Wallet setup walkthrough
- **[Identity Verification](/demos/identity-verification.md)** - KYC/AML process demo
- **[Investor Qualification](/demos/investor-qualification.md)** - Accreditation verification

### Capital Markets Protocol

- **[Protocol Overview](/protocol/README.md)** - Multi-chain smart contract framework for capital markets
- **[Smart Contracts](/protocol/contracts.md)** - Contract architecture and APIs
- **[ERC-7752 Lot Tokens](/protocol/erc-7752.md)** - Revolutionary token standard for capital markets
- **[Deployment Guide](/protocol/deployment.md)** - Protocol deployment instructions
- **[API Reference](/protocol/api.md)** - Integration endpoints
- **[Testing Guide](/protocol/testing.md)** - Testing frameworks and strategies

### Compliance Framework

- **[Compliance Overview](/compliance/README.md)** - Regulatory compliance architecture
- **[Framework Implementation](/compliance/framework-implementation.md)** - Technical implementation
- **[Attestation Schemas](/compliance/attestation-schemas.md)** - KYC, qualification, jurisdiction
- **[Verification Process](/compliance/verification-process.md)** - Document processing

### CMX Network

- **[Network Overview](/cmx-network/README.md)** - L2 network architecture
- **[CMX Token](/cmx-network/token.md)** - Native token economics
- **[Network Governance](/cmx-network/governance.md)** - Participation and voting
- **[Bridges & Interoperability](/cmx-network/bridges.md)** - Cross-chain operations
- **[Validator Guide](/cmx-network/validators.md)** - Run network validators

### Self-Hosted Infrastructure

- **[Infrastructure Overview](/infrastructure/README.md)** - AWS deployment architecture
- **[Prerequisites](/infrastructure/prerequisites.md)** - System requirements
- **[Quick Start Guide](/infrastructure/quickstart.md)** - Fast deployment
- **[Repository Setup](/infrastructure/setup.md)** - Project configuration
- **[Secrets Configuration](/infrastructure/secrets.md)** - Security setup
- **[Installation Guide](/infrastructure/installation.md)** - Step-by-step deployment
- **[Optional Services](/infrastructure/optional-services.md)** - Third-party integrations
- **[SOC Compliance](/infrastructure/soc-compliance.md)** - Compliance guidelines

### Developer Resources

- **[Developer Overview](/developers/README.md)** - Start building with CapSign

### Security & Compliance

- **[SOC Compliance](/security/soc.md)** - SOC 1/2/3 certification guide

### Pricing & Business

- **[Pricing Overview](/pricing/README.md)** - Service tiers and CMX token discounts
- **[Self-Hosted vs Managed](/pricing/comparison.md)** - Deployment option comparison
- **[Enterprise Solutions](/pricing/enterprise.md)** - Custom institutional offerings
- **[CMX Token Utility](/pricing/token-utility.md)** - Token economics and savings

### Help & Support

- **[FAQ](/help/faq.md)** - Frequently asked questions
- **[Glossary](/GLOSSARY.md)** - Technical terms and definitions

---

## Additional Resources

- **[GitHub Organization](https://github.com/capsign)** - Source code repositories
- **[Discord Community](https://discord.gg/gSmnZ9wmNv)** - Real-time chat and support
- **[Blog](https://blog.capsign.com)** - Technical insights and updates
- **[Status Page](https://status.capsign.com)** - Service availability

## Contributing to Documentation

Found an error or want to improve the docs? We welcome contributions!

1. **[Fork the docs repository](https://github.com/capsign/docs)**
2. **Make your changes** and test locally
3. **Submit a pull request** with a clear description
4. **Our team will review** and merge approved changes

### Local Development

This documentation is built with GitBook. For local development and preview:

```bash
# Clone the repository
git clone https://github.com/capsign/docs.git
cd docs

# For simple preview, use any static server
python3 -m http.server 8000
# Open http://localhost:8000

# Or use docsify for a better experience
npm i docsify-cli -g
docsify serve docs
```

---

**Ready to get started?** Begin with our [Choose Your Path](/getting-started/README.md) guide to determine the best deployment option for your organization.
