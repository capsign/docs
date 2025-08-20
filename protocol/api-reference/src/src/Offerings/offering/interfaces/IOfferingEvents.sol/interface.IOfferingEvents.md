# IOfferingEvents
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Offerings/offering/interfaces/IOfferingEvents.sol)

Domain-specific events for Offering diamonds


## Events
### OfferingCreated

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

### OfferingClosed

```solidity
event OfferingClosed();
```

### OfferingCancelled

```solidity
event OfferingCancelled();
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
event OfferingBaseURIUpdated(string newURI);
```

### DocumentRegistryUpdated

```solidity
event DocumentRegistryUpdated(address indexed oldRegistry, address indexed newRegistry);
```

### OfferingOperatorRoleRequested

```solidity
event OfferingOperatorRoleRequested(address indexed offering, address indexed asset, address indexed authority);
```

