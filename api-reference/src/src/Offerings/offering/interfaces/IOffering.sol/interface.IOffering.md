# IOffering
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Offerings/offering/interfaces/IOffering.sol)


## Events
### OfferingCreated
Events


```solidity
event OfferingCreated(
    address indexed entity,
    address indexed asset,
    address indexed paymentCurrency,
    uint256 paymentDecimals,
    uint256 pricePerToken,
    uint256 minInvestment,
    uint256 investmentDeadline,
    uint256 targetAmount,
    uint256 maxAmount,
    string baseURI
);
```

### OfferingStatusChanged

```solidity
event OfferingStatusChanged(Status newStatus);
```

### OfferingInvested

```solidity
event OfferingInvested(
    address indexed investor, uint256 investmentId, uint256 amount, uint256 pricePerToken, bytes32 instanceHash
);
```

### OfferingInvestmentCountersigned

```solidity
event OfferingInvestmentCountersigned(address indexed investor, uint256 investmentId, uint256 amount);
```

### OfferingPricePerTokenUpdated

```solidity
event OfferingPricePerTokenUpdated(uint256 newPrice);
```

### OfferingMinInvestmentUpdated

```solidity
event OfferingMinInvestmentUpdated(uint256 newMin);
```

### OfferingInvestmentDeadlineUpdated

```solidity
event OfferingInvestmentDeadlineUpdated(uint256 newDeadline);
```

### OfferingTargetAmountUpdated

```solidity
event OfferingTargetAmountUpdated(uint256 newTarget);
```

### OfferingMaxAmountUpdated

```solidity
event OfferingMaxAmountUpdated(uint256 newMax);
```

### OfferingBaseURIUpdated

```solidity
event OfferingBaseURIUpdated(string newBaseURI);
```

### OfferingClosed

```solidity
event OfferingClosed();
```

### InvestmentMade

```solidity
event InvestmentMade(address indexed investor, uint256 amountInvested, uint256 tokensIssued);
```

### InvestmentCountersigned

```solidity
event InvestmentCountersigned(uint256 indexed investmentId, address indexed investor);
```

### OffChainInvestmentRecorded

```solidity
event OffChainInvestmentRecorded(
    uint256 indexed investmentId, address indexed investor, uint256 amount, uint256 investmentDate, bytes32 instanceHash
);
```

### FundsWithdrawn

```solidity
event FundsWithdrawn(address indexed recipient, uint256 amount);
```

### OfferingCancelled

```solidity
event OfferingCancelled();
```

### InvestmentRejected

```solidity
event InvestmentRejected(uint256 indexed investmentId, address indexed investor, uint256 amount);
```

### InvestmentRefunded

```solidity
event InvestmentRefunded(uint256 indexed investmentId, address indexed investor, uint256 amount);
```

### DocumentAdded

```solidity
event DocumentAdded(bytes32 indexed hash, bool isTemplate);
```

### DocumentRemoved

```solidity
event DocumentRemoved(bytes32 indexed hash, bool isTemplate);
```

### DocumentRegistryUpdated

```solidity
event DocumentRegistryUpdated(address indexed oldRegistry, address indexed newRegistry);
```

### TemplateApproved

```solidity
event TemplateApproved(bytes32 indexed templateHash);
```

### TemplateDisapproved

```solidity
event TemplateDisapproved(bytes32 indexed templateHash);
```

## Errors
### InvalidStatus

```solidity
error InvalidStatus();
```

### NotEntity

```solidity
error NotEntity();
```

### NotManager

```solidity
error NotManager();
```

### NotInvestor

```solidity
error NotInvestor();
```

### AmountBelowMinimum

```solidity
error AmountBelowMinimum();
```

### AmountAboveMaximum

```solidity
error AmountAboveMaximum();
```

### CapExceeded

```solidity
error CapExceeded();
```

### InvestmentPeriodEnded

```solidity
error InvestmentPeriodEnded();
```

### AlreadyInvested

```solidity
error AlreadyInvested();
```

### ComplianceCheckFailed

```solidity
error ComplianceCheckFailed();
```

### SignatureVerificationFailed

```solidity
error SignatureVerificationFailed();
```

### InvalidSignature

```solidity
error InvalidSignature();
```

### InvalidAgreementHash

```solidity
error InvalidAgreementHash();
```

### InvestmentNotFound

```solidity
error InvestmentNotFound();
```

### AlreadyCountersigned

```solidity
error AlreadyCountersigned();
```

### TransferFailed

```solidity
error TransferFailed();
```

### ZeroAddress

```solidity
error ZeroAddress();
```

### ZeroAmount

```solidity
error ZeroAmount();
```

### InvalidPrice

```solidity
error InvalidPrice();
```

### CallerNotFactory

```solidity
error CallerNotFactory();
```

### FundsNotAvailable

```solidity
error FundsNotAvailable();
```

### InvalidInvestmentId

```solidity
error InvalidInvestmentId();
```

### DocumentNotFound

```solidity
error DocumentNotFound();
```

## Structs
### Investment

```solidity
struct Investment {
    address investor;
    uint256 amount;
    uint256 pricePerToken;
    uint256 investmentDate;
    bool countersigned;
    bool isOffChain;
    bytes32 instanceHash;
}
```

## Enums
### Status

```solidity
enum Status {
    Active,
    Completed,
    Cancelled
}
```

### DocumentType

```solidity
enum DocumentType {
    SIGNATURE,
    DISCLOSURE
}
```

### DocumentAccessLevel

```solidity
enum DocumentAccessLevel {
    Public,
    Participants,
    Investors
}
```

