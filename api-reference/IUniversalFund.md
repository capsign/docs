# IUniversalFund
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Funds/fund/interfaces/IUniversalFund.sol)

Interface for Universal Fund operations

*Defines the standard interface for fund operations including configuration, status management, and core functionality*


## Functions
### initialize

Initialize the fund with configuration


```solidity
function initialize(
    string memory name,
    string memory symbol,
    FundConfig memory config,
    address capitalAccountRegistry,
    address distributionEngine,
    address unitToken
) external;
```

### getFundConfig

Get fund configuration


```solidity
function getFundConfig() external view returns (FundConfig memory);
```

### getFundStatus

Get current fund status


```solidity
function getFundStatus() external view returns (FundStatus);
```

### setFundStatus

Update fund status


```solidity
function setFundStatus(FundStatus newStatus) external;
```

### name

Get fund name


```solidity
function name() external view returns (string memory);
```

### symbol

Get fund symbol


```solidity
function symbol() external view returns (string memory);
```

### getTotalCommitments

Get total committed capital


```solidity
function getTotalCommitments() external view returns (uint256);
```

### getTotalContributions

Get total contributed capital


```solidity
function getTotalContributions() external view returns (uint256);
```

### getUnfundedCommitments

Get unfunded commitments


```solidity
function getUnfundedCommitments() external view returns (uint256);
```

### getPortfolioValue

Get current portfolio value


```solidity
function getPortfolioValue() external view returns (uint256);
```

### canExtendTerm

Check if fund term can be extended


```solidity
function canExtendTerm() external view returns (bool);
```

### extendFundTerm

Extend fund term


```solidity
function extendFundTerm(uint256 additionalMonths) external;
```

### pause

Pause fund operations


```solidity
function pause() external;
```

### unpause

Unpause fund operations


```solidity
function unpause() external;
```

### paused

Check if fund is paused


```solidity
function paused() external view returns (bool);
```

## Events
### FundConfigUpdated
Emitted when fund configuration is updated


```solidity
event FundConfigUpdated(FundConfig config);
```

### FundStatusChanged
Emitted when fund status changes


```solidity
event FundStatusChanged(FundStatus oldStatus, FundStatus newStatus);
```

### InvestmentMade
Emitted when an investment is made


```solidity
event InvestmentMade(bytes32 indexed investmentId, address indexed target, uint256 amount);
```

### InvestmentExited
Emitted when an investment is exited


```solidity
event InvestmentExited(bytes32 indexed investmentId, uint256 exitValue);
```

### NAVUpdated
Emitted when NAV is updated


```solidity
event NAVUpdated(uint256 newNAV, uint256 navPerShare);
```

## Structs
### FundConfig
Fund configuration parameters


```solidity
struct FundConfig {
    FundType fundType;
    FundStructure structure;
    uint256 targetSize;
    uint256 hardCap;
    uint256 managementFeeBps;
    uint256 carriedInterestBps;
    uint256 preferredReturnBps;
    uint256 fundTermMonths;
    uint256 extensionMonths;
    address denominationAsset;
    uint256 minimumCommitment;
    bool allowRedemptions;
    uint256 redemptionNoticeDays;
}
```

### Investment
Investment tracking structure


```solidity
struct Investment {
    bytes32 id;
    address asset;
    uint256 amount;
    address target;
    InvestmentType investmentType;
    uint256 timestamp;
    bool isActive;
    uint256 currentValue;
    int256 unrealizedPnL;
    bytes metadata;
}
```

### InvestmentLimits
Investment limits and constraints


```solidity
struct InvestmentLimits {
    uint256 maxAllocation;
    uint256 maxSingleInvestment;
    bool isActive;
}
```

## Enums
### FundType
Fund types supported by the protocol


```solidity
enum FundType {
    OPEN_END,
    CLOSED_END
}
```

### FundStructure
Fund structures supported by the protocol


```solidity
enum FundStructure {
    LP_GP,
    LLC,
    CORPORATION,
    TRUST
}
```

### FundStatus
Current operational status of the fund


```solidity
enum FundStatus {
    FUNDRAISING,
    ACTIVE,
    CLOSED,
    LIQUIDATING
}
```

### InvestmentType
Types of investments the fund can make


```solidity
enum InvestmentType {
    EQUITY,
    DEBT,
    HYBRID,
    REAL_ESTATE,
    CRYPTO
}
```

