# CMX Protocol API Reference

Complete API reference for interacting with CMX Protocol smart contracts.

## SDK Overview

The CMX Protocol SDK provides a TypeScript/JavaScript interface for all protocol interactions.

### Installation

```bash
npm install @capsign/sdk
# or
yarn add @capsign/sdk
```

### Basic Setup

```typescript
import { CMXClient, Network } from "@capsign/sdk";

const client = new CMXClient({
  network: Network.BASE_SEPOLIA,
  privateKey: process.env.PRIVATE_KEY,
  // Optional: custom RPC endpoint
  rpcUrl: "https://sepolia.base.org",
  // Optional: etherscan API key for verification
  etherscanApiKey: process.env.BASESCAN_API_KEY,
});
```

## Asset Management

### Create Assets

#### Create Share Class

```typescript
const shareClass = await client.assets.createShareClass({
  name: "Example Corp Class A",
  symbol: "EXMP-A",
  totalShares: 1000000,
  votingRights: true,
  dividendRights: true,
  transferRestrictions: {
    requireKYC: true,
    accreditedOnly: false,
    lockupPeriod: 0,
  },
  governanceConfig: {
    votingPower: 1,
    quorumPercentage: 25,
    votingPeriod: 604800, // 7 days
  },
});

console.log(`Share class created at: ${shareClass.address}`);
```

#### Create Off-Chain Asset

```typescript
const asset = await client.assets.createOffChainAsset({
  name: "Manhattan Office Building",
  symbol: "MOB-001",
  assetType: "REAL_ESTATE",
  totalValue: ethers.utils.parseEther("10000000"), // $10M
  jurisdiction: "US-NY",
  compliance: {
    requireKYC: true,
    accreditedOnly: true,
    qualifiedPurchaser: true,
  },
  metadata: {
    location: "Manhattan, New York",
    propertyType: "Office Building",
    sqft: 50000,
    ipfsHash: "QmX...", // IPFS hash of detailed asset information
  },
});
```

### Asset Operations

#### Transfer Assets

```typescript
// Simple transfer (if compliance allows)
const transferTx = await client.assets.transfer({
  assetAddress: "0x123...",
  to: "0x456...",
  amount: ethers.utils.parseEther("100"),
});

// Transfer with compliance check
const complianceTransfer = await client.assets.transferWithCompliance({
  assetAddress: "0x123...",
  to: "0x456...",
  amount: ethers.utils.parseEther("100"),
  complianceData: {
    kycVerified: true,
    accreditationLevel: "QUALIFIED_PURCHASER",
    jurisdictionApproved: true,
  },
});
```

#### Asset Queries

```typescript
// Get asset information
const assetInfo = await client.assets.getAssetInfo("0x123...");
console.log(assetInfo);
// Returns:
// {
//   name: 'Example Corp Class A',
//   symbol: 'EXMP-A',
//   totalSupply: '1000000',
//   votingRights: true,
//   dividendRights: true,
//   ...
// }

// Get user balance
const balance = await client.assets.getBalance("0x123...", "0x456...");

// Get transfer restrictions
const restrictions = await client.assets.getTransferRestrictions("0x123...");
```

## Trading & Markets

### Order Book Trading

#### Place Orders

```typescript
// Place buy order
const buyOrder = await client.trading.placeBuyOrder({
  market: "0x789...",
  asset: "0x123...",
  amount: ethers.utils.parseEther("100"),
  price: ethers.utils.parseEther("10"), // $10 per share
  orderType: "LIMIT",
  timeInForce: "GTC", // Good Till Cancelled
});

// Place sell order
const sellOrder = await client.trading.placeSellOrder({
  market: "0x789...",
  asset: "0x123...",
  amount: ethers.utils.parseEther("50"),
  price: ethers.utils.parseEther("11"),
  orderType: "LIMIT",
  timeInForce: "FOK", // Fill Or Kill
});
```

#### Order Management

```typescript
// Cancel order
await client.trading.cancelOrder("0x789...", buyOrder.orderId);

// Get order status
const orderStatus = await client.trading.getOrder("0x789...", buyOrder.orderId);

// Get order book
const orderBook = await client.trading.getOrderBook("0x789...", "0x123...");
console.log(orderBook);
// Returns:
// {
//   bids: [
//     { price: '10.00', amount: '100', orderId: '123' },
//     { price: '9.95', amount: '200', orderId: '124' }
//   ],
//   asks: [
//     { price: '10.05', amount: '150', orderId: '125' },
//     { price: '10.10', amount: '75', orderId: '126' }
//   ]
// }
```

### Auction Markets

#### Create Auction

```typescript
const auction = await client.auctions.createAuction({
  asset: "0x123...",
  amount: ethers.utils.parseEther("1000"),
  auctionType: "ENGLISH", // or 'DUTCH', 'SEALED_BID'
  startingPrice: ethers.utils.parseEther("5"),
  reservePrice: ethers.utils.parseEther("8"),
  duration: 86400, // 24 hours
  bidIncrement: ethers.utils.parseEther("0.1"),
});
```

#### Bid on Auction

```typescript
const bid = await client.auctions.placeBid({
  auctionId: auction.auctionId,
  amount: ethers.utils.parseEther("9.5"),
});
```

### OTC Trading

#### Create OTC Offer

```typescript
const otcOffer = await client.otc.createOffer({
  asset: "0x123...",
  amount: ethers.utils.parseEther("5000"),
  price: ethers.utils.parseEther("12"),
  counterparty: "0x456...", // Optional: specific counterparty
  expiry: Math.floor(Date.now() / 1000) + 3600, // 1 hour
});
```

#### Accept OTC Offer

```typescript
await client.otc.acceptOffer(otcOffer.offerId);
```

## Fund Management

### Create Investment Fund

```typescript
const fund = await client.funds.createUniversalFund({
  name: "Growth Equity Fund I",
  symbol: "GEF-I",
  fundType: "HEDGE_FUND",
  strategy: "GROWTH_EQUITY",
  baseCurrency: "USD",
  managementFee: 200, // 2% (in basis points)
  performanceFee: 2000, // 20%
  highWaterMark: true,
  lockupPeriod: 31536000, // 1 year
  redemptionFrequency: "QUARTERLY",
  minimumInvestment: ethers.utils.parseEther("1000000"), // $1M
  gateProvisions: {
    enabled: true,
    maxRedemptionPercentage: 25, // 25% per quarter
  },
});
```

### Fund Operations

#### Investor Operations

```typescript
// Deposit (invest in fund)
const investment = await client.funds.deposit({
  fundAddress: fund.address,
  amount: ethers.utils.parseEther("5000000"), // $5M investment
  investor: "0x456...",
});

// Withdraw (redeem from fund)
const redemption = await client.funds.withdraw({
  fundAddress: fund.address,
  shares: ethers.utils.parseEther("1000"), // Fund shares to redeem
  investor: "0x456...",
});

// Get investor position
const position = await client.funds.getInvestorPosition(
  fund.address,
  "0x456..."
);
```

#### Fund Administration

```typescript
// Calculate NAV
const nav = await client.funds.calculateNAV(fund.address);

// Process performance fees
await client.funds.processPerformanceFees(fund.address);

// Get fund performance
const performance = await client.funds.getPerformanceMetrics(fund.address);
console.log(performance);
// Returns:
// {
//   totalReturn: '15.5', // 15.5%
//   annualizedReturn: '12.3',
//   sharpeRatio: '1.45',
//   maxDrawdown: '8.2',
//   volatility: '18.7'
// }
```

## Compliance & Attestations

### KYC/AML Operations

#### Create Attestation

```typescript
const kycAttestation = await client.compliance.createKYCAttestation({
  subject: "0x456...",
  verificationLevel: "ENHANCED",
  accreditationStatus: "ACCREDITED_INVESTOR",
  jurisdictions: ["US", "EU"],
  expiryDate: Math.floor(Date.now() / 1000) + 31536000, // 1 year
  verifierSignature: "0x...", // Signature from authorized verifier
});
```

#### Verify Compliance

```typescript
// Check if address meets compliance requirements
const complianceCheck = await client.compliance.verifyCompliance({
  address: "0x456...",
  requirements: {
    kycRequired: true,
    accreditedInvestor: true,
    qualifiedPurchaser: false,
    jurisdiction: "US",
  },
});

console.log(complianceCheck.approved); // true/false
console.log(complianceCheck.reasons); // Array of compliance issues if any
```

### Document Management

#### Register Document

```typescript
const documentHash = await client.documents.register({
  documentType: "PROSPECTUS",
  ipfsHash: "QmX...",
  title: "Growth Equity Fund I Prospectus",
  version: "1.0",
  signatories: ["0x123...", "0x456..."],
});
```

#### Verify Document

```typescript
const isValid = await client.documents.verify(documentHash);
const documentInfo = await client.documents.getInfo(documentHash);
```

## Governance

### Proposal Management

#### Create Proposal

```typescript
const proposal = await client.governance.createProposal({
  title: "Increase Management Fee Cap",
  description:
    "Proposal to increase the maximum management fee from 2% to 2.5%",
  targets: ["0x123..."], // Contract addresses to call
  values: [0], // ETH values to send
  calldatas: ["0x..."], // Encoded function calls
  votingPeriod: 604800, // 7 days
});
```

#### Vote on Proposal

```typescript
// Vote in favor
await client.governance.vote({
  proposalId: proposal.proposalId,
  support: true,
  reason: "This change will help attract top talent",
});

// Delegate voting power
await client.governance.delegate({
  delegatee: "0x456...", // Address to delegate voting power to
});
```

#### Execute Proposal

```typescript
// After voting period ends and proposal passes
await client.governance.execute(proposal.proposalId);
```

## Utilities & Helpers

### Price Feeds

```typescript
// Get current price
const price = await client.prices.getPrice("0x123...");

// Get historical prices
const historicalPrices = await client.prices.getHistoricalPrices("0x123...", {
  from: startTimestamp,
  to: endTimestamp,
  interval: "DAILY",
});

// Set price (if authorized)
await client.prices.setPrice("0x123...", ethers.utils.parseEther("15.50"));
```

### Escrow Services

```typescript
// Create escrow
const escrow = await client.escrow.create({
  asset: "0x123...",
  amount: ethers.utils.parseEther("1000"),
  beneficiary: "0x456...",
  releaseConditions: {
    timelock: Math.floor(Date.now() / 1000) + 2592000, // 30 days
    approvalRequired: true,
    approvers: ["0x789..."],
  },
});

// Release escrow
await client.escrow.release(escrow.escrowId);
```

### Batch Operations

```typescript
// Batch multiple operations in one transaction
const batchTx = await client.batch.execute([
  {
    target: '0x123...',
    calldata: client.assets.interface.encodeFunctionData('transfer', ['0x456...', '100'])
  },
  {
    target: '0x789...',
    calldata: client.trading.interface.encodeFunctionData('placeBuyOrder', [...])
  }
]);
```

## Event Subscriptions

### Subscribe to Events

```typescript
// Subscribe to asset transfers
client.events.subscribe("AssetTransfer", {
  filter: {
    asset: "0x123...",
  },
  callback: (event) => {
    console.log(
      `Transfer: ${event.from} -> ${event.to}, Amount: ${event.amount}`
    );
  },
});

// Subscribe to trade executions
client.events.subscribe("TradeExecuted", {
  filter: {
    market: "0x789...",
  },
  callback: (event) => {
    console.log(`Trade executed: ${event.amount} at ${event.price}`);
  },
});

// Subscribe to governance events
client.events.subscribe("ProposalCreated", {
  callback: (event) => {
    console.log(`New proposal: ${event.description}`);
  },
});
```

## Error Handling

```typescript
import { CMXError, ErrorCode } from "@capsign/sdk";

try {
  await client.assets.transfer({
    assetAddress: "0x123...",
    to: "0x456...",
    amount: ethers.utils.parseEther("100"),
  });
} catch (error) {
  if (error instanceof CMXError) {
    switch (error.code) {
      case ErrorCode.INSUFFICIENT_BALANCE:
        console.log("Insufficient balance for transfer");
        break;
      case ErrorCode.COMPLIANCE_VIOLATION:
        console.log("Transfer violates compliance rules");
        break;
      case ErrorCode.TRANSFER_RESTRICTED:
        console.log("Asset has transfer restrictions");
        break;
      default:
        console.log(`Protocol error: ${error.message}`);
    }
  } else {
    console.log(`Unexpected error: ${error.message}`);
  }
}
```

## Configuration Options

### Advanced Client Configuration

```typescript
const client = new CMXClient({
  network: Network.BASE_MAINNET,
  privateKey: process.env.PRIVATE_KEY,

  // Gas configuration
  gasConfig: {
    gasLimit: 1000000,
    maxFeePerGas: ethers.utils.parseUnits("20", "gwei"),
    maxPriorityFeePerGas: ethers.utils.parseUnits("2", "gwei"),
  },

  // Retry configuration
  retryConfig: {
    maxRetries: 3,
    retryDelay: 1000,
    exponentialBackoff: true,
  },

  // Logging configuration
  logging: {
    level: "INFO",
    includeTransactionDetails: true,
  },

  // Cache configuration
  cache: {
    enabled: true,
    ttl: 300000, // 5 minutes
  },
});
```

### Environment-Specific Settings

```typescript
// Development configuration
const devClient = new CMXClient({
  network: Network.BASE_SEPOLIA,
  privateKey: process.env.DEV_PRIVATE_KEY,
  debug: true,
  simulateTransactions: true,
});

// Production configuration
const prodClient = new CMXClient({
  network: Network.BASE_MAINNET,
  privateKey: process.env.PROD_PRIVATE_KEY,
  confirmations: 3, // Wait for 3 confirmations
  timeout: 60000, // 60 second timeout
  fallbackRpcUrls: [
    "https://mainnet.base.org",
    "https://base-mainnet.infura.io/v3/YOUR-PROJECT-ID",
  ],
});
```

## Rate Limiting & Best Practices

### Efficient API Usage

```typescript
// Use batch operations when possible
const results = await client.batch.query([
  () => client.assets.getBalance("0x123...", "0x456..."),
  () => client.funds.getNAV("0x789..."),
  () => client.trading.getOrderBook("0xabc...", "0x123..."),
]);

// Cache frequently accessed data
const cachedPrice =
  (await client.cache.get("price:0x123...")) ||
  (await client.prices.getPrice("0x123..."));

// Use event subscriptions instead of polling
client.events.subscribe("AssetTransfer", {
  filter: { asset: "0x123..." },
  callback: updateUI,
});
```

This comprehensive API reference provides all the tools needed to build sophisticated capital markets applications on the CMX Protocol.
