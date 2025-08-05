# CMX Protocol Smart Contracts

Complete reference for all smart contracts in the CMX Protocol ecosystem.

## Architecture Overview

The CMX Protocol uses the **Diamond Pattern (EIP-2535)** for upgradeable, modular smart contracts. This architecture provides:

- **Modularity**: Each feature is a separate facet
- **Upgradability**: Add/remove/replace functionality without changing addresses
- **Gas Efficiency**: Only load required functionality
- **Size Limits**: Bypass 24KB contract size limits

## Diamond Factories

Central factory contracts that create and manage diamond-based assets and systems.

### WalletFactory

Creates smart wallets with configurable facets for different user types.

**Address**: `0x...` (Base Sepolia)

**Key Functions**:

```solidity
function createWallet(
    bytes32 salt,
    address[] calldata facets,
    bytes[] calldata facetCalls
) external returns (address wallet);

function getWallet(address owner, bytes32 salt) external view returns (address);
```

**Facets Available**:

- **ERC4337Facet** - Account abstraction support
- **MultisigFacet** - Multi-signature functionality
- **DelegationFacet** - Voting delegation
- **ComplianceFacet** - KYC/AML integration

### AssetFactory

Creates tokenized assets including securities, RWAs, and share classes.

**Address**: `0x...` (Base Sepolia)

**Key Functions**:

```solidity
function createShareClass(
    string calldata name,
    string calldata symbol,
    uint256 totalShares,
    ShareClassConfig calldata config
) external returns (address shareClass);

function createOffChainAsset(
    string calldata name,
    string calldata symbol,
    AssetConfig calldata config
) external returns (address asset);
```

**Asset Types**:

- **ShareClass** - Corporate equity instruments
- **OffChainAsset** - Real-world asset representation
- **Bond** - Debt instruments
- **Derivative** - Financial derivatives

### FundFactory

Creates investment funds with configurable strategies and governance.

**Address**: `0x...` (Base Sepolia)

**Key Functions**:

```solidity
function createUniversalFund(
    string calldata name,
    string calldata symbol,
    FundConfig calldata config
) external returns (address fund);
```

**Fund Types**:

- **UniversalFund** - General investment fund
- **HedgeFund** - Alternative investment strategies
- **MutualFund** - Regulated mutual funds
- **ETF** - Exchange-traded funds

### OfferingFactory

Creates securities offerings compliant with various regulations.

**Address**: `0x...` (Base Sepolia)

**Key Functions**:

```solidity
function createRule506b(
    address asset,
    OfferingConfig calldata config
) external returns (address offering);

function createRule506c(
    address asset,
    OfferingConfig calldata config
) external returns (address offering);
```

**Offering Types**:

- **Rule506b** - Private placement (accredited investors)
- **Rule506c** - General solicitation allowed
- **Rule701** - Employee stock option plans
- **RegA+** - Mini public offerings

## Access Management

Unified access control system providing role-based permissions across the entire protocol.

### GlobalAccessManager

Central access control contract managing all protocol permissions.

**Address**: `0x...` (Base Sepolia)

**Key Functions**:

```solidity
function grantRole(bytes32 role, address account, uint32 delay) external;
function revokeRole(bytes32 role, address account) external;
function hasRole(bytes32 role, address account) external view returns (bool);
```

**System Roles**:

- **ADMIN_ROLE** - Protocol administration
- **OPERATOR_ROLE** - Operational functions
- **COMPLIANCE_ROLE** - KYC/AML operations
- **AUDIT_ROLE** - Read-only audit access

## Attestations

Decentralized identity verification and compliance system based on Ethereum Attestation Service (EAS).

### AttestationRegistry

Manages attestation schemas and verification processes.

**Address**: `0x...` (Base Sepolia)

**Key Functions**:

```solidity
function createSchema(
    string calldata schema,
    bool revocable
) external returns (bytes32 schemaId);

function attest(
    bytes32 schemaId,
    address recipient,
    bytes calldata data
) external returns (bytes32 attestationId);
```

**Attestation Types**:

- **KYC Verification** - Identity verification
- **Accredited Investor** - Investment qualification
- **Qualified Purchaser** - High net worth verification
- **Professional Investor** - Professional status

### ERC20PaymentResolver

Handles payment requirements for creating attestations.

**Key Functions**:

```solidity
function pay(
    bytes32 schemaId,
    uint256 amount,
    address token
) external returns (bool);
```

## Tokenization

Framework for creating and managing tokenized real-world assets and securities.

### Share Classes

Corporate equity instruments with configurable rights and restrictions.

**Key Features**:

- **Voting Rights** - Configurable voting power
- **Dividend Rights** - Automatic dividend distribution
- **Transfer Restrictions** - Compliance-based transfer controls
- **Anti-Dilution** - Protection mechanisms

**Functions**:

```solidity
function transfer(address to, uint256 amount) external returns (bool);
function vote(uint256 proposalId, bool support) external;
function claimDividends() external;
```

### Off-Chain Assets

Real-world asset representation with on-chain management.

**Asset Types**:

- **Real Estate** - Property tokenization
- **Commodities** - Physical commodity representation
- **Art & Collectibles** - High-value collectible assets
- **Infrastructure** - Infrastructure project tokens

## Trading Markets

Multiple trading venue types for different market structures and regulatory requirements.

### Order Book Markets

Traditional exchange-style order matching with full order book depth.

**Key Functions**:

```solidity
function placeBuyOrder(
    address asset,
    uint256 amount,
    uint256 price
) external returns (uint256 orderId);

function placeSellOrder(
    address asset,
    uint256 amount,
    uint256 price
) external returns (uint256 orderId);

function cancelOrder(uint256 orderId) external;
```

### Auction Markets

Price discovery through various auction mechanisms.

**Auction Types**:

- **English Auction** - Ascending price auction
- **Dutch Auction** - Descending price auction
- **Sealed Bid** - Private bid submission
- **Batch Auction** - Periodic clearing

### OTC Markets

Over-the-counter trading for private negotiations and large block trades.

**Key Functions**:

```solidity
function createOTCOffer(
    address asset,
    uint256 amount,
    uint256 price,
    address counterparty
) external returns (uint256 offerId);

function acceptOTCOffer(uint256 offerId) external;
```

## Funds Management

Investment fund operations including NAV calculation, fee management, and investor relations.

### Universal Fund

Flexible fund structure supporting various investment strategies.

**Key Functions**:

```solidity
function deposit(uint256 amount) external returns (uint256 shares);
function withdraw(uint256 shares) external returns (uint256 amount);
function calculateNAV() external view returns (uint256);
```

**Features**:

- **Performance Fees** - Management and performance fee calculation
- **High Water Mark** - Performance fee protection
- **Redemption Gates** - Liquidity management
- **Side Pockets** - Illiquid asset management

## Governance

Decentralized governance system for protocol upgrades and parameter changes.

### DAO Governance

Multi-signature and voting-based governance for critical protocol decisions.

**Key Functions**:

```solidity
function propose(
    address[] calldata targets,
    uint256[] calldata values,
    bytes[] calldata calldatas,
    string calldata description
) external returns (uint256 proposalId);

function vote(uint256 proposalId, bool support) external;
function execute(uint256 proposalId) external;
```

**Governance Parameters**:

- **Proposal Threshold** - Minimum tokens to propose
- **Voting Period** - Duration of voting
- **Quorum** - Minimum participation required
- **Time Lock** - Execution delay for security

## Accounting Ledgers

Double-entry bookkeeping system for transparent financial reporting.

### Ledger System

Enterprise-grade accounting with automated journal entries.

**Key Functions**:

```solidity
function createJournalEntry(
    uint256 debitAccount,
    uint256 creditAccount,
    uint256 amount,
    string calldata description
) external;

function getBalance(uint256 accountId) external view returns (int256);
```

**Account Types**:

- **Assets** - Cash, securities, receivables
- **Liabilities** - Payables, debt obligations
- **Equity** - Owner's equity, retained earnings
- **Income** - Fee income, investment gains
- **Expenses** - Operating expenses, management fees

## Billing & Subscriptions

Payment and subscription management system for protocol usage fees.

### Subscription Manager

Handles tiered subscriptions and usage-based billing.

**Key Functions**:

```solidity
function subscribe(uint256 tierId) external;
function payUsageFee(uint256 amount) external;
function cancelSubscription() external;
```

**Subscription Tiers**:

- **Basic** - Limited functionality
- **Professional** - Full feature access
- **Enterprise** - Custom limits and support
- **Institutional** - White-glove service

## Payment Abstraction

Gas abstraction and payment systems for improved user experience.

### CMX Paymaster

ERC-4337 paymaster supporting CMX token gas payments.

**Key Functions**:

```solidity
function depositFor(address account) external payable;
function withdrawTo(address account, uint256 amount) external;
```

**Features**:

- **CMX Gas Payments** - Pay gas fees with CMX tokens
- **Sponsored Transactions** - Gasless transactions for users
- **Batch Operations** - Multiple operations in one transaction

## Document Management

Secure document storage and verification system for compliance and legal requirements.

### Document Registry

IPFS-based document storage with on-chain verification.

**Key Functions**:

```solidity
function registerDocument(
    bytes32 documentHash,
    string calldata ipfsHash,
    DocumentType docType
) external;

function verifyDocument(bytes32 documentHash) external view returns (bool);
```

**Document Types**:

- **Legal Documents** - Contracts, agreements
- **Compliance Reports** - Audit reports, certifications
- **Financial Statements** - Balance sheets, income statements
- **Prospectuses** - Investment offering documents

## Utilities

Supporting contracts and utilities for enhanced functionality.

### Price Feeds

Oracle-based price feeds for asset valuation and trading.

**Key Functions**:

```solidity
function getPrice(address asset) external view returns (uint256);
function updatePrice(address asset, uint256 price) external;
```

### Escrow

Secure escrow services for conditional transfers and settlements.

**Key Functions**:

```solidity
function createEscrow(
    address asset,
    uint256 amount,
    address beneficiary,
    uint256 releaseTime
) external returns (uint256 escrowId);

function releaseEscrow(uint256 escrowId) external;
```

## Events and Monitoring

Comprehensive event logging for transparency and monitoring.

### Event Types

**Asset Events**:

- `AssetCreated(address indexed asset, string name, string symbol)`
- `Transfer(address indexed from, address indexed to, uint256 amount)`
- `Approval(address indexed owner, address indexed spender, uint256 amount)`

**Trading Events**:

- `OrderPlaced(uint256 indexed orderId, address indexed trader, uint256 amount, uint256 price)`
- `OrderMatched(uint256 indexed buyOrderId, uint256 indexed sellOrderId, uint256 price, uint256 amount)`
- `OrderCancelled(uint256 indexed orderId, address indexed trader)`

**Governance Events**:

- `ProposalCreated(uint256 indexed proposalId, address indexed proposer, string description)`
- `VoteCast(uint256 indexed proposalId, address indexed voter, bool support, uint256 votes)`
- `ProposalExecuted(uint256 indexed proposalId)`

## Security Considerations

### Access Control

- All privileged functions require appropriate roles
- Multi-signature requirements for administrative actions
- Time-delayed execution for critical changes

### Upgrade Safety

- Diamond cuts require multi-signature approval
- Upgrade delays for community review
- Emergency pause mechanisms

### Audit Trail

- Complete event logging for all operations
- Immutable transaction history
- Compliance-ready reporting

## Gas Optimization

### Efficient Patterns

- Batch operations to reduce transaction costs
- Lazy evaluation for expensive computations
- Optimized storage layouts

### Cost Estimates

- Asset creation: ~500K gas
- Simple transfer: ~100K gas
- Order placement: ~200K gas
- Governance vote: ~150K gas

## Integration Patterns

### Common Workflows

**Asset Tokenization**:

1. Create attestation schema for asset type
2. Verify asset ownership via attestation
3. Create tokenized asset via AssetFactory
4. Configure transfer restrictions and compliance rules

**Investment Fund Setup**:

1. Create fund via FundFactory
2. Configure fee structure and governance
3. Set up accounting ledger
4. Launch marketing and investor onboarding

**Securities Offering**:

1. Create share class or asset
2. Set up offering via OfferingFactory
3. Configure investor verification requirements
4. Launch offering and manage subscriptions

This comprehensive smart contract system provides all the infrastructure needed for compliant, enterprise-grade capital markets operations on blockchain.
