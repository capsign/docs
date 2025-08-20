# ICapitalAccountRegistry
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Funds/fund/interfaces/ICapitalAccountRegistry.sol)

Interface for capital account registry operations

*Defines the standard interface for managing investor capital accounts*


## Functions
### createAccount

Create a new capital account


```solidity
function createAccount(address investor, InvestorType investorType, uint256 commitment) external;
```

### recordCommitment

Record additional commitment


```solidity
function recordCommitment(address investor, uint256 amount) external;
```

### recordContribution

Record capital contribution


```solidity
function recordContribution(address investor, uint256 amount, uint256 callNumber) external;
```

### recordDistribution

Record distribution


```solidity
function recordDistribution(address investor, uint256 amount, uint256 distributionType) external;
```

### deductManagementFee

Deduct management fee


```solidity
function deductManagementFee(address investor, uint256 amount) external;
```

### allocateCarriedInterest

Allocate carried interest


```solidity
function allocateCarriedInterest(address gp, uint256 amount) external;
```

### updatePreferredReturn

Update preferred return


```solidity
function updatePreferredReturn(address investor, uint256 amount) external;
```

### closeAccount

Close an account


```solidity
function closeAccount(address investor) external;
```

### getAccount

Get capital account information


```solidity
function getAccount(address investor) external view returns (CapitalAccount memory);
```

### getAccountEntries

Get account entries


```solidity
function getAccountEntries(address investor, uint256 offset, uint256 limit)
    external
    view
    returns (AccountEntry[] memory);
```

### getTotalCommitments

Get total commitments


```solidity
function getTotalCommitments() external view returns (uint256);
```

### getTotalContributions

Get total contributions


```solidity
function getTotalContributions() external view returns (uint256);
```

### getTotalDistributions

Get total distributions


```solidity
function getTotalDistributions() external view returns (uint256);
```

### getUnfundedCommitments

Get unfunded commitments


```solidity
function getUnfundedCommitments() external view returns (uint256);
```

### getInvestorsByType

Get investors by type


```solidity
function getInvestorsByType(InvestorType investorType) external view returns (address[] memory);
```

### calculateOwnershipPercentage

Calculate ownership percentage


```solidity
function calculateOwnershipPercentage(address investor) external view returns (uint256);
```

### calculateProRataShare

Calculate pro-rata share


```solidity
function calculateProRataShare(address investor, uint256 totalAmount) external view returns (uint256);
```

### isInvestor

Check if address is investor


```solidity
function isInvestor(address account) external view returns (bool);
```

### getActiveInvestorCount

Get active investor count


```solidity
function getActiveInvestorCount() external view returns (uint256);
```

## Events
### AccountCreated
Emitted when a new capital account is created


```solidity
event AccountCreated(address indexed investor, InvestorType investorType, uint256 commitment);
```

### CapitalCommitted
Emitted when capital is committed


```solidity
event CapitalCommitted(address indexed investor, uint256 amount);
```

### CapitalContributed
Emitted when capital is contributed


```solidity
event CapitalContributed(address indexed investor, uint256 amount, uint256 callNumber);
```

### DistributionRecorded
Emitted when distribution is recorded


```solidity
event DistributionRecorded(address indexed investor, uint256 amount, uint256 distributionType);
```

### ManagementFeeDeducted
Emitted when management fee is deducted


```solidity
event ManagementFeeDeducted(address indexed investor, uint256 amount);
```

### CarriedInterestAllocated
Emitted when carried interest is allocated


```solidity
event CarriedInterestAllocated(address indexed gp, uint256 amount);
```

### AccountClosed
Emitted when an account is closed


```solidity
event AccountClosed(address indexed investor);
```

## Structs
### CapitalAccount
Capital account information


```solidity
struct CapitalAccount {
    address investor;
    InvestorType investorType;
    uint256 commitment;
    uint256 totalContributions;
    uint256 totalDistributions;
    uint256 unfundedCommitment;
    uint256 preferredReturn;
    uint256 profitShare;
    bool isActive;
}
```

### AccountEntry
Account entry for audit trail


```solidity
struct AccountEntry {
    uint256 timestamp;
    AccountEntryType entryType;
    uint256 amount;
    uint256 balance;
    string description;
}
```

## Enums
### InvestorType
Types of investors


```solidity
enum InvestorType {
    LP,
    GP
}
```

### AccountEntryType
Types of account entries


```solidity
enum AccountEntryType {
    COMMITMENT,
    CONTRIBUTION,
    DISTRIBUTION,
    FEE,
    CARRY
}
```

