# CMX Protocol Smart Contracts

Complete reference for all smart contracts deployed in the CMX Protocol ecosystem on Base Sepolia.

## Deployment Information

**Network**: Base Sepolia (Chain ID: 84532)  
**Block Explorer**: https://sepolia.basescan.org  
**Latest Deployment**: July 31, 2025

### Core System Contracts

| Contract            | Address                                      | Description                         |
| ------------------- | -------------------------------------------- | ----------------------------------- |
| GlobalAccessManager | `0x81574D4c3BC62d6A9Ff166608af328824a0694bd` | Central access control system       |
| FacetRegistry       | `0x49Df9c09A41eCAC13203E70Db04F1025C8982B20` | Diamond facet registry              |
| Factory             | TBD                                          | Main diamond factory contract       |
| SubscriptionManager | TBD                                          | Payment and subscription management |

## Architecture Overview

The CMX Protocol uses the **Diamond Pattern (EIP-2535)** for upgradeable, modular smart contracts. Each functional area is implemented as separate facets that can be added, updated, or removed from diamond contracts.

### Diamond Core Facets

These facets provide the fundamental diamond functionality:

#### DiamondCutFacet

**Address**: `0xd805e4480b4227d60887d2c70f292b3888eb4681`

Handles diamond upgrades by adding, replacing, or removing facets.

**Key Functions**:

```solidity
function diamondCut(
    FacetCut[] calldata _diamondCut,
    address _init,
    bytes calldata _calldata
) external;
```

#### DiamondLoupeFacet

**Address**: `0xc5478314b0d2e2b7447dd9d5a2ff6a750451bb78`

Provides introspection into diamond structure and facets.

**Key Functions**:

```solidity
function facets() external view returns (Facet[] memory facets_);
function facetFunctionSelectors(address _facet) external view returns (bytes4[] memory);
function facetAddresses() external view returns (address[] memory);
function facetAddress(bytes4 _functionSelector) external view returns (address);
```

#### OwnableFacet

**Address**: `0x5c474dcaf95f2763e47f463d6f0de24801a3d895`

Provides ownership management for diamond contracts.

#### AccessControlFacet

**Address**: `0xb171c70c7d084c85d6f1a1aff4ea8d4c70efded3`

Implements role-based access control within diamond contracts.

## Asset Management Facets

### ShareClassFacet

Implements corporate equity shares with voting and dividend rights.

**Features**:

- ERC20-compatible token functionality
- Voting rights management
- Dividend distribution
- Transfer restrictions
- Corporate actions

### OffChainAssetFacet

Manages tokenized real-world assets with off-chain components.

**Asset Types Supported**:

- Real estate properties
- Commodities
- Art and collectibles
- Infrastructure assets
- Private debt instruments

### AssetCoreFacet

Core asset functionality shared across all asset types.

### AssetAdminFacet

Administrative functions for asset management.

### AssetConfigFacet

Configuration and parameter management for assets.

### AssetDeploymentFacet

Factory functionality for creating new asset instances.

### AssetPaymasterFacet

Gas abstraction for asset-related transactions.

## Wallet & Account Management

### CoreWalletFacet

Core smart wallet functionality with account abstraction support.

**Features**:

- ERC4337 account abstraction
- Multi-signature support
- Transaction batching
- Gas payment flexibility

### WebAuthnProxyFacet

Biometric authentication integration for enhanced security.

### AccountManagementFacet

User account lifecycle management.

### WalletConfigFacet

Wallet configuration and personalization settings.

### WalletDeploymentFacet

**Address**: `0xaee4f63c3bab3feffcbf02eb7dda336e70f1dede`

Factory for creating new smart wallet instances.

### WalletPaymasterFacet

Gas sponsorship and payment abstraction for wallet operations.

## Asset Deployment System

### AssetDeploymentFacet

**Address**: `0x55aa4a7c25e85c7716c6895d41277afd62996ce5`

Factory functionality for creating new tokenized asset instances.

### FundDeploymentFacet

**Address**: `0x57671c9848fee178ff752976123cbb42e0f40744`

Factory for creating new investment fund instances.

### LedgerDeploymentFacet

**Address**: `0xe4fb3753029aeb0ccb0d051014578753e7a9babe`

Factory for creating accounting ledger instances.

### OfferingDeploymentFacet

**Address**: `0xeecf66ebb9458d8c564c7bb23f08f28a13939b39`

Factory for creating securities offering instances.

## Governance Facets

### GovernanceDeploymentFacet

**Address**: `0x176565846d05f2cfd3bcc832a3aa43c28f541efb`

Factory for creating governance instances.

### GovernanceRegistryFacet

**Address**: `0x7a8abffc159b69b0d7e44dcc2404aba5391af0d0`

Governance proposal and voting registry.

### GovernanceConfigFacet

**Address**: `0xbc28e385de188bcf4b18d91995a81fcc466be5b5`

Governance parameter configuration.

### GovernancePaymasterFacet

**Address**: `0xc78072109fbd65ad58e0608edb873595b7b026e3`

Gas abstraction for governance operations.

### DelegationFacet

Voting power delegation functionality.

## Ledger & Accounting Facets

### LedgerConfigFacet

**Address**: `0x73956370c5873749e477e0d3a60db878f34dbe00`

Ledger configuration and chart of accounts.

### LedgerFacet

Double-entry bookkeeping implementation.

### LedgerPaymasterFacet

Gas abstraction for ledger operations.

## Investment Funds

### UniversalFundCoreFacet

Core investment fund functionality supporting multiple fund types.

**Fund Types**:

- Hedge funds
- Mutual funds
- Private equity funds
- Real estate investment trusts (REITs)

### InvestmentFacet

Investment execution and portfolio management.

### DistributionFacet

Distribution payments to fund investors.

### AdvancedDistributionFacet

Complex distribution strategies and waterfall calculations.

### FundUnitFacet

Fund share/unit management and tracking.

### CapitalCallFacet

Capital call management for private funds.

### CapitalAccountFacet

Capital account tracking for partnership-style funds.

## Trading & Transfer Facets

### TransferFacet

Handles compliant asset transfers with regulatory checks.

**Features**:

- KYC/AML compliance verification
- Transfer restrictions enforcement
- Regulatory jurisdiction checks
- Accredited investor verification

### TransactionFacet

Core transaction processing and settlement.

### LotManagementFacet

Manages trading lots and position tracking.

### SanctionsFacet

OFAC and international sanctions screening.

### Rule144Facet

Implements SEC Rule 144 compliance for restricted securities.

## Employee Stock Options (Rule 701)

### Rule701ConfigFacet

Configuration for employee stock option plans.

### Rule701DeploymentFacet

Factory for creating option plan instances.

### ExerciseFacet

Option exercise functionality.

### VestingFacet

Vesting schedule management and tracking.

### LockupFacet

Post-exercise lockup period management.

### EsoFacet

Employee stock option core functionality.

### WarrantFacet

Warrant instrument management.

### SarFacet

Stock Appreciation Rights (SAR) implementation.

## Compliance & Attestations

### AttestationCoreFacet

Core attestation functionality for identity verification.

### AttestationAdminFacet

Administrative functions for attestation management.

### AttestationQueryFacet

Query interface for attestation verification.

### AttestationFacet

User-facing attestation interface.

### SchemaManagementFacet

Manages attestation schemas for different verification types.

**Attestation Types**:

- KYC verification (Basic, Enhanced)
- Accredited investor status
- Qualified purchaser status
- Professional investor designation
- Jurisdiction-specific compliance

### AttestationRegistryFactory

Factory for creating attestation registry instances.

### ComplianceFacet

Regulatory compliance checks and enforcement.

## Document Management

### DocumentCoreFacet

Core document storage and verification functionality.

### DocumentAdminFacet

Administrative document management functions.

### DocumentSigningFacet

Digital signature and document execution.

### DocumentTemplateFacet

Document template management for common legal documents.

### DocumentRegistryFactory

Factory for creating document registry instances.

## Securities Offerings

### OfferingConfigFacet

Configuration for securities offerings under various regulations.

### OfferingPaymasterFacet

Gas abstraction for offering-related transactions.

**Supported Offering Types**:

- Rule 506(b) private placements
- Rule 506(c) general solicitation offerings
- Rule 701 employee stock option plans
- Regulation A+ mini-public offerings

## Access Management

### IdentityAccessManagerCoreFacet

Core access management for asset issuers.

### IdentityAccessManagerConfigFacet

Configuration for issuer-specific access controls.

### IdentityAccessManagerDeploymentFacet

Factory for creating issuer access manager instances.

### IdentityAccessManagerGovernanceFacet

Governance functionality for issuer access management.

### IdentityAccessManagerSubscriptionFacet

Subscription management for issuer services.

### IdentityAccessManagerWalletFacet

Wallet integration for issuer access management.

### IdentityAccessManagerPaymasterFacet

Gas abstraction for access management operations.

## Payment & Gas Abstraction

### PaymasterManagementFacet

Core paymaster functionality for gas abstraction.

### PaymasterValidationFacet

Validation logic for sponsored transactions.

**Payment Features**:

- CMX token gas payments
- Sponsored transactions
- Batch operations
- Cross-chain payments

## Query & Data Access

### QueryFacet

General-purpose query interface for protocol data.

**Query Capabilities**:

- Asset information and balances
- Compliance status verification
- Transaction history
- Governance proposal status
- Fund performance metrics

## Utility Contracts

### CMX Token

The native utility token of the CMX Network.

**Contract**: `CMX.sol`

**Features**:

- ERC20 standard compliance
- Governance token functionality
- Gas payment utility
- Staking mechanisms
- Cross-chain bridge support

### USD Stablecoin

USD-pegged stablecoin for protocol operations.

**Contract**: `USD.sol`

### TokenSale

Token sale and distribution contract.

**Contract**: `TokenSale.sol`

### SafeFacet

Multi-signature wallet functionality integration.

## Factory System

### AttestationRegistryFactory

Creates new attestation registry instances for different jurisdictions or use cases.

### DocumentRegistryFactory

Creates document registry instances for secure document management.

### Factory (Main)

Primary factory contract coordinating all other factories.

## Integration Patterns

### Diamond Upgrade Process

1. **Deploy New Facet**: Deploy the new facet contract
2. **Prepare Diamond Cut**: Create the facet cut data structure
3. **Execute Upgrade**: Call `diamondCut()` with appropriate permissions
4. **Verify Upgrade**: Confirm new functionality is available

### Asset Creation Workflow

1. **Compliance Setup**: Create required attestation schemas
2. **Asset Deployment**: Use AssetDeploymentFacet to create new asset
3. **Configuration**: Configure asset parameters via AssetConfigFacet
4. **Compliance Integration**: Link compliance requirements
5. **Trading Setup**: Configure transfer restrictions and market access

### Fund Management Workflow

1. **Fund Creation**: Deploy fund diamond via FundDeploymentFacet
2. **Strategy Configuration**: Set investment strategy parameters
3. **Compliance Setup**: Configure investor requirements
4. **Accounting Integration**: Link to accounting ledger
5. **Investor Onboarding**: Process investor applications and KYC

## Security Considerations

### Access Control

- All facets inherit from AccessControlFacet for role-based permissions
- GlobalAccessManager provides protocol-wide access coordination
- Multi-signature requirements for sensitive operations

### Upgrade Safety

- Diamond cuts require appropriate admin roles
- Time delays for critical upgrades
- Emergency pause mechanisms available

### Compliance Integration

- All asset transfers go through compliance checks
- Automated sanctions screening
- Regulatory jurisdiction enforcement

## Gas Optimization

### Facet Design

- Minimal storage in diamond proxy
- Efficient function selector routing
- Batch operations where possible

### Payment Abstraction

- CMX token gas payments reduce costs
- Sponsored transactions for user experience
- Batch operations for efficiency

## Event Monitoring

### Key Events

- Asset creation and transfers
- Compliance status changes
- Governance proposal lifecycle
- Fund performance updates
- System admin actions

This comprehensive smart contract system provides enterprise-grade capital markets infrastructure with regulatory compliance, security, and scalability built in from the ground up.
