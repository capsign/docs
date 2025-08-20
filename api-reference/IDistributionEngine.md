# IDistributionEngine
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Funds/fund/interfaces/IDistributionEngine.sol)

Interface for distribution engine operations

*Defines the standard interface for managing fund distributions with waterfall logic*


## Functions
### initializeDistributionConfig

Initialize distribution configuration


```solidity
function initializeDistributionConfig(
    WaterfallModel model,
    uint256 preferredReturnRate,
    uint256 catchupPercentage,
    uint256 carriedInterestSplit,
    bool compoundPreferredReturn,
    uint256 hurdleRate
) external;
```

### setDistributionConfig

Update distribution configuration


```solidity
function setDistributionConfig(
    WaterfallModel model,
    uint256 preferredReturnRate,
    uint256 catchupPercentage,
    uint256 carriedInterestSplit,
    bool compoundPreferredReturn,
    uint256 hurdleRate
) external;
```

### calculateDistribution

Calculate distribution amounts


```solidity
function calculateDistribution(uint256 totalAmount, bool isFinalDistribution)
    external
    view
    returns (address[] memory recipients, uint256[] memory amounts, DistributionType[] memory distributionTypes);
```

### executeDistribution

Execute distribution with waterfall logic


```solidity
function executeDistribution(uint256 totalAmount, bool isFinalDistribution)
    external
    returns (address[] memory recipients, uint256[] memory amounts, DistributionType[] memory distributionTypes);
```

### accruePreferredReturn

Accrue preferred return for a period


```solidity
function accruePreferredReturn(uint256 periodDays) external;
```

### updateWaterfallState

Update waterfall state


```solidity
function updateWaterfallState(uint256 contributions, uint256 distributions, DistributionType distributionType)
    external;
```

### getDistributionConfig

Get distribution configuration


```solidity
function getDistributionConfig() external view returns (DistributionConfig memory);
```

### getWaterfallState

Get waterfall state


```solidity
function getWaterfallState() external view returns (WaterfallState memory);
```

### calculateCarriedInterest

Calculate carried interest for a GP


```solidity
function calculateCarriedInterest(uint256 profits, address gp) external view returns (uint256);
```

### calculatePreferredReturn

Calculate preferred return for an investor


```solidity
function calculatePreferredReturn(address investor, uint256 periodDays) external view returns (uint256);
```

### getRemainingPreferredReturn

Get remaining preferred return owed


```solidity
function getRemainingPreferredReturn() external view returns (uint256);
```

### isHurdleRateAchieved

Check if hurdle rate is achieved


```solidity
function isHurdleRateAchieved() external view returns (bool);
```

### getNextDistributionType

Get next distribution type in waterfall


```solidity
function getNextDistributionType() external view returns (DistributionType);
```

### simulateDistribution

Simulate distribution without executing


```solidity
function simulateDistribution(uint256 amount, bool isFinalDistribution)
    external
    view
    returns (
        uint256 capitalReturn,
        uint256 preferredReturn,
        uint256 gpCatchup,
        uint256 carriedInterest,
        uint256 residualProfits
    );
```

## Events
### DistributionCalculated
Emitted when distribution is calculated


```solidity
event DistributionCalculated(uint256 totalAmount, uint256 recipientCount);
```

### DistributionExecuted
Emitted when distribution is executed


```solidity
event DistributionExecuted(address indexed recipient, uint256 amount, DistributionType distributionType);
```

### PreferredReturnAccrued
Emitted when preferred return is accrued


```solidity
event PreferredReturnAccrued(uint256 amount, uint256 totalAccrued);
```

### CarriedInterestEarned
Emitted when carried interest is earned


```solidity
event CarriedInterestEarned(address indexed gp, uint256 amount);
```

### WaterfallModelUpdated
Emitted when waterfall model is updated


```solidity
event WaterfallModelUpdated(WaterfallModel oldModel, WaterfallModel newModel);
```

### DistributionConfigUpdated
Emitted when distribution configuration is updated


```solidity
event DistributionConfigUpdated(WaterfallModel model, uint256 preferredReturnRate, uint256 carriedInterestSplit);
```

### HurdleRateAchieved
Emitted when hurdle rate is achieved


```solidity
event HurdleRateAchieved(uint256 totalReturn, uint256 hurdleRate);
```

## Structs
### DistributionConfig
Distribution configuration


```solidity
struct DistributionConfig {
    WaterfallModel model;
    uint256 preferredReturnRate;
    uint256 catchupPercentage;
    uint256 carriedInterestSplit;
    bool compoundPreferredReturn;
    uint256 hurdleRate;
}
```

### WaterfallState
Waterfall state tracking


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

## Enums
### WaterfallModel
Waterfall models for distribution calculations


```solidity
enum WaterfallModel {
    AMERICAN,
    EUROPEAN,
    HYBRID
}
```

### DistributionType
Types of distributions


```solidity
enum DistributionType {
    RETURN_OF_CAPITAL,
    PREFERRED_RETURN,
    GP_CATCHUP,
    CARRIED_INTEREST,
    RESIDUAL_PROFITS
}
```

