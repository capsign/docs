# UniversalFundStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Funds/fund/storage/UniversalFundStorage.sol)

**Author:**
CMX Protocol Team

Diamond storage library for UniversalFund state

*Implements the diamond storage pattern to avoid storage collisions between facets.
All fund state is stored in a single struct at a deterministic storage slot.
Storage Layout:
- Fund configuration and metadata
- Component contract references
- Financial tracking (commitments, contributions, distributions)
- Capital call management
- Investor and portfolio company tracking
- Access control and status management*


## State Variables
### UNIVERSAL_FUND_STORAGE_POSITION
Storage slot for fund data (keccak256 hash for uniqueness)


```solidity
bytes32 constant UNIVERSAL_FUND_STORAGE_POSITION = keccak256("capsign.universalfund.storage");
```


## Functions
### layout

Get the storage layout for the fund

*Uses assembly to return the storage struct at the predetermined slot*


```solidity
function layout() internal pure returns (Layout storage l);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`l`|`Layout`|The storage layout struct|


## Structs
### Layout
Main storage layout for the fund diamond

*All fund facets access this shared storage structure*


```solidity
struct Layout {
    string name;
    string symbol;
    IUniversalFund.FundConfig fundConfig;
    IUniversalFund.FundStatus fundStatus;
    ICapitalAccountRegistry capitalAccountRegistry;
    IDistributionEngine distributionEngine;
    IFundUnit unitToken;
    uint256 fundStartDate;
    uint256 fundEndDate;
    uint256 extensionsUsed;
    uint256 totalCommitments;
    uint256 totalContributions;
    uint256 totalDistributions;
    uint256 portfolioValue;
    uint256 lastManagementFeeDate;
    uint256 totalManagementFees;
    uint256 currentCallNumber;
    mapping(uint256 => CapitalCallData) capitalCalls;
    mapping(address => bool) isInvestor;
    mapping(address => bool) isPortfolioCompany;
    mapping(bytes32 => mapping(address => bool)) roles;
    bool paused;
    mapping(bytes32 => IUniversalFund.Investment) investments;
    bytes32[] activeInvestments;
    uint256 totalInvestments;
    mapping(address => uint256) portfolioAllocations;
    mapping(address => IUniversalFund.InvestmentLimits) investmentLimits;
    uint256 totalInvested;
    int256 unrealizedPnL;
    int256 realizedPnL;
    uint256 navPerShare;
    uint256 totalShares;
    mapping(uint256 => Distribution) distributions;
    uint256 totalDistributionsCount;
    mapping(uint256 => mapping(address => uint256)) investorDistributions;
    mapping(uint256 => mapping(address => bool)) distributionPaid;
    mapping(address => uint256) totalInvestorDistributions;
    uint256 totalCapitalReturned;
    uint256 totalPreferredReturnPaid;
    uint256 totalProfitDistributed;
    uint256 fundActivationDate;
    address[] investors;
    mapping(address => uint256) commitments;
    mapping(address => CapitalAccount) capitalAccounts;
    mapping(address => AccountEntry[]) accountEntries;
    mapping(uint8 => address[]) investorsByType;
    uint256 activeInvestorCount;
    DistributionConfig distributionConfig;
    WaterfallState waterfallState;
}
```

### CapitalCallData
Capital call data structure

*Separate struct to avoid mapping of structs with mappings in main Layout*


```solidity
struct CapitalCallData {
    uint256 callNumber;
    uint256 totalAmount;
    uint256 dueDate;
    uint256 purposeHash;
    bool isComplete;
    mapping(address => bool) hasPaid;
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`callNumber`|`uint256`|Unique identifier for this capital call|
|`totalAmount`|`uint256`|Total amount being called from all investors|
|`dueDate`|`uint256`|Deadline for investor contributions|
|`purposeHash`|`uint256`|Hash of IPFS document describing call purpose|
|`isComplete`|`bool`|Whether the capital call has been completed|
|`hasPaid`|`mapping(address => bool)`|Mapping of investor addresses to payment status|

### Distribution
Distribution data structure

*Stores information about fund distributions to investors*


```solidity
struct Distribution {
    uint256 id;
    uint8 distributionType;
    uint256 totalAmount;
    uint256 recordDate;
    uint256 paymentDate;
    bool isComplete;
    uint256 returnOfCapital;
    uint256 preferredReturn;
    uint256 profitDistribution;
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`id`|`uint256`|Unique identifier for this distribution|
|`distributionType`|`uint8`|Type of distribution (return of capital, profit, etc.)|
|`totalAmount`|`uint256`|Total amount being distributed|
|`recordDate`|`uint256`|Date for determining eligible investors|
|`paymentDate`|`uint256`|Date when distribution will be paid|
|`isComplete`|`bool`|Whether the distribution has been completed|
|`returnOfCapital`|`uint256`|Amount allocated as return of capital|
|`preferredReturn`|`uint256`|Amount allocated as preferred return|
|`profitDistribution`|`uint256`|Amount allocated as profit distribution|

### CapitalAccount
Capital account data structure for tracking investor details

*Stores comprehensive investor account information*


```solidity
struct CapitalAccount {
    address investor;
    uint8 investorType;
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

*Records all account transactions for transparency*


```solidity
struct AccountEntry {
    uint256 timestamp;
    uint8 entryType;
    uint256 amount;
    uint256 balance;
    string description;
}
```

### DistributionConfig
Distribution configuration for advanced waterfall calculations

*Configuration for complex distribution models*


```solidity
struct DistributionConfig {
    uint8 model;
    uint256 preferredReturnRate;
    uint256 catchupPercentage;
    uint256 carriedInterestSplit;
    bool compoundPreferredReturn;
    uint256 hurdleRate;
}
```

### WaterfallState
Waterfall state tracking for distribution calculations

*Tracks current state of distribution waterfall*


```solidity
struct WaterfallState {
    uint256 totalContributions;
    uint256 totalDistributed;
    uint256 returnedCapital;
    uint256 distributedProfits;
    uint256 accruedPreferredReturn;
    uint256 paidPreferredReturn;
    uint256 gpCatchup;
    uint256 carriedInterestPaid;
}
```

