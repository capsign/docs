# AdvancedDistributionFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Funds/fund/facets/AdvancedDistributionFacet.sol)

**Inherits:**
AccessControl

**Author:**
CMX Protocol Team

Diamond facet for advanced distribution waterfall calculations

*This facet handles complex distribution logic including:
- Multiple waterfall models (American, European, Hybrid)
- GP catchup calculations
- Preferred return accrual over time
- Hurdle rate logic
- Distribution simulation and modeling
Key Features:
- American waterfall (deal-by-deal carry)
- European waterfall (whole fund carry)
- Hybrid waterfall models
- Time-based preferred return accrual
- GP catchup mechanisms
- Hurdle rate thresholds
- Distribution simulation
- Performance analytics
Waterfall Models:
- American: GP gets carry on each profitable exit
- European: GP gets carry only after all capital + preferred return
- Hybrid: Custom combination of American and European*


## State Variables
### FUND_ROLE
Role for fund operations


```solidity
bytes32 public constant FUND_ROLE = keccak256("FUND_ROLE");
```


### OPERATOR_ROLE
Role for operators


```solidity
bytes32 public constant OPERATOR_ROLE = keccak256("OPERATOR_ROLE");
```


### GP_ROLE
Role for general partner


```solidity
bytes32 public constant GP_ROLE = keccak256("GP_ROLE");
```


### BASIS_POINTS
Basis points constant (10000 = 100%)


```solidity
uint256 private constant BASIS_POINTS = 10000;
```


### DAYS_PER_YEAR
Days per year for calculations


```solidity
uint256 private constant DAYS_PER_YEAR = 365;
```


### WATERFALL_AMERICAN

```solidity
uint8 public constant WATERFALL_AMERICAN = 0;
```


### WATERFALL_EUROPEAN

```solidity
uint8 public constant WATERFALL_EUROPEAN = 1;
```


### WATERFALL_HYBRID

```solidity
uint8 public constant WATERFALL_HYBRID = 2;
```


### DIST_RETURN_OF_CAPITAL

```solidity
uint8 public constant DIST_RETURN_OF_CAPITAL = 0;
```


### DIST_PREFERRED_RETURN

```solidity
uint8 public constant DIST_PREFERRED_RETURN = 1;
```


### DIST_GP_CATCHUP

```solidity
uint8 public constant DIST_GP_CATCHUP = 2;
```


### DIST_CARRIED_INTEREST

```solidity
uint8 public constant DIST_CARRIED_INTEREST = 3;
```


### DIST_RESIDUAL_PROFITS

```solidity
uint8 public constant DIST_RESIDUAL_PROFITS = 4;
```


## Functions
### onlyFundOrGP


```solidity
modifier onlyFundOrGP();
```

### onlyOperatorOrGP


```solidity
modifier onlyOperatorOrGP();
```

### initializeDistributionConfig

Initialize the advanced distribution engine

*Sets up distribution configuration and waterfall parameters*


```solidity
function initializeDistributionConfig(
    uint8 model,
    uint256 preferredReturnRate,
    uint256 catchupPercentage,
    uint256 carriedInterestSplit,
    bool compoundPreferredReturn,
    uint256 hurdleRate
) external onlyOperatorOrGP;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`model`|`uint8`|Waterfall model (0=American, 1=European, 2=Hybrid)|
|`preferredReturnRate`|`uint256`|Annual preferred return rate in basis points|
|`catchupPercentage`|`uint256`|GP catchup percentage (e.g., 100 for 100%)|
|`carriedInterestSplit`|`uint256`|GP share after catchup (e.g., 20 for 20%)|
|`compoundPreferredReturn`|`bool`|Whether to compound preferred return|
|`hurdleRate`|`uint256`|Minimum return before carry (in basis points)|


### setDistributionConfig

Update distribution configuration

*Updates the waterfall configuration parameters*


```solidity
function setDistributionConfig(
    uint8 model,
    uint256 preferredReturnRate,
    uint256 catchupPercentage,
    uint256 carriedInterestSplit,
    bool compoundPreferredReturn,
    uint256 hurdleRate
) external onlyOperatorOrGP;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`model`|`uint8`|New waterfall model|
|`preferredReturnRate`|`uint256`|New preferred return rate|
|`catchupPercentage`|`uint256`|New catchup percentage|
|`carriedInterestSplit`|`uint256`|New carried interest split|
|`compoundPreferredReturn`|`bool`|Whether to compound preferred return|
|`hurdleRate`|`uint256`|New hurdle rate|


### calculateDistribution

Calculate distribution amounts without executing

*Performs waterfall calculation based on current configuration*


```solidity
function calculateDistribution(uint256 totalAmount, bool isFinalDistribution)
    external
    view
    returns (address[] memory recipients, uint256[] memory amounts, uint8[] memory distributionTypes);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`totalAmount`|`uint256`|Total amount to distribute|
|`isFinalDistribution`|`bool`|Whether this is the final distribution|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`recipients`|`address[]`|Array of recipient addresses|
|`amounts`|`uint256[]`|Array of distribution amounts|
|`distributionTypes`|`uint8[]`|Array of distribution types|


### executeDistribution

Execute a distribution with waterfall logic

*Calculates and records distribution according to waterfall*


```solidity
function executeDistribution(uint256 totalAmount, bool isFinalDistribution)
    external
    onlyFundOrGP
    returns (address[] memory recipients, uint256[] memory amounts, uint8[] memory distributionTypes);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`totalAmount`|`uint256`|Total amount to distribute|
|`isFinalDistribution`|`bool`|Whether this is the final distribution|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`recipients`|`address[]`|Array of recipient addresses|
|`amounts`|`uint256[]`|Array of distribution amounts|
|`distributionTypes`|`uint8[]`|Array of distribution types|


### accruePreferredReturn

Accrue preferred return for a period

*Calculates and records preferred return accrual*


```solidity
function accruePreferredReturn(uint256 periodDays) external onlyFundOrGP;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`periodDays`|`uint256`|Number of days to accrue|


### updateWaterfallState

Update waterfall state manually

*Updates state for external contributions/distributions*


```solidity
function updateWaterfallState(uint256 contributions, uint256 distributions, uint8 distributionType)
    external
    onlyFundOrGP;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`contributions`|`uint256`|New contributions|
|`distributions`|`uint256`|New distributions|
|`distributionType`|`uint8`|Type of distribution|


### getDistributionConfig

Get current distribution configuration


```solidity
function getDistributionConfig() external view returns (UniversalFundStorage.DistributionConfig memory config);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`config`|`UniversalFundStorage.DistributionConfig`|Current distribution configuration|


### getWaterfallState

Get current waterfall state


```solidity
function getWaterfallState() external view returns (UniversalFundStorage.WaterfallState memory state);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`state`|`UniversalFundStorage.WaterfallState`|Current waterfall state|


### calculateCarriedInterest

Calculate carried interest for a GP


```solidity
function calculateCarriedInterest(uint256 profits, address gp) external view returns (uint256 carriedInterest);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`profits`|`uint256`|Total profits|
|`gp`|`address`|GP address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`carriedInterest`|`uint256`|Carried interest amount|


### calculatePreferredReturn

Calculate preferred return for an investor over a period


```solidity
function calculatePreferredReturn(address investor, uint256 periodDays)
    external
    view
    returns (uint256 preferredReturn);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`periodDays`|`uint256`|Number of days|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`preferredReturn`|`uint256`|Preferred return amount|


### getRemainingPreferredReturn

Get remaining preferred return owed


```solidity
function getRemainingPreferredReturn() external view returns (uint256 remaining);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`remaining`|`uint256`|Remaining preferred return amount|


### isHurdleRateAchieved

Check if hurdle rate has been achieved


```solidity
function isHurdleRateAchieved() public view returns (bool achieved);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`achieved`|`bool`|True if hurdle rate is achieved|


### getNextDistributionType

Get the next distribution type in the waterfall


```solidity
function getNextDistributionType() external view returns (uint8 distributionType);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`distributionType`|`uint8`|Next distribution type|


### simulateDistribution

Simulate a distribution without executing


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
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|Amount to distribute|
|`isFinalDistribution`|`bool`|Whether this is final distribution|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`capitalReturn`|`uint256`|Amount going to capital return|
|`preferredReturn`|`uint256`|Amount going to preferred return|
|`gpCatchup`|`uint256`|Amount going to GP catchup|
|`carriedInterest`|`uint256`|Amount going to carried interest|
|`residualProfits`|`uint256`|Amount going to residual profits|


### _calculateDistribution

Internal distribution calculation with waterfall logic


```solidity
function _calculateDistribution(uint256 totalAmount, bool isFinalDistribution)
    internal
    view
    returns (address[] memory recipients, uint256[] memory amounts, uint8[] memory distributionTypes);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`totalAmount`|`uint256`|Total amount to distribute|
|`isFinalDistribution`|`bool`|Whether this is final distribution|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`recipients`|`address[]`|Array of recipient addresses|
|`amounts`|`uint256[]`|Array of distribution amounts|
|`distributionTypes`|`uint8[]`|Array of distribution types|


### _calculateCapitalReturn

Calculate capital return amount


```solidity
function _calculateCapitalReturn(uint256 availableAmount) internal view returns (uint256 capitalReturn);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`availableAmount`|`uint256`|Available amount for distribution|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`capitalReturn`|`uint256`|Amount for capital return|


### _calculatePreferredReturn

Calculate preferred return amount


```solidity
function _calculatePreferredReturn(uint256 availableAmount) internal view returns (uint256 preferredReturn);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`availableAmount`|`uint256`|Available amount for distribution|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`preferredReturn`|`uint256`|Amount for preferred return|


### _calculateGPCatchup

Calculate GP catchup amount


```solidity
function _calculateGPCatchup(uint256 availableAmount) internal view returns (uint256 catchupAmount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`availableAmount`|`uint256`|Available amount for distribution|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`catchupAmount`|`uint256`|Amount for GP catchup|


### _calculateTargetCatchup

Calculate target GP catchup amount


```solidity
function _calculateTargetCatchup() internal view returns (uint256 targetCatchup);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`targetCatchup`|`uint256`|Target catchup amount|


### _distributeProRata

Distribute amount pro-rata among investors


```solidity
function _distributeProRata(
    address[] memory recipients,
    uint256[] memory amounts,
    uint8[] memory distributionTypes,
    uint256 startIndex,
    address[] memory investors,
    uint256 amount,
    uint8 distType
) internal view returns (uint256 nextIndex);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`recipients`|`address[]`|Recipients array|
|`amounts`|`uint256[]`|Amounts array|
|`distributionTypes`|`uint8[]`|Distribution types array|
|`startIndex`|`uint256`|Starting index in arrays|
|`investors`|`address[]`|Array of investor addresses|
|`amount`|`uint256`|Amount to distribute|
|`distType`|`uint8`|Distribution type|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`nextIndex`|`uint256`|Next available index in arrays|


### _distributePreferred

Distribute preferred return based on amounts owed


```solidity
function _distributePreferred(
    address[] memory recipients,
    uint256[] memory amounts,
    uint8[] memory distributionTypes,
    uint256 startIndex,
    address[] memory investors,
    uint256 amount
) internal view returns (uint256 nextIndex);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`recipients`|`address[]`|Recipients array|
|`amounts`|`uint256[]`|Amounts array|
|`distributionTypes`|`uint8[]`|Distribution types array|
|`startIndex`|`uint256`|Starting index in arrays|
|`investors`|`address[]`|Array of investor addresses|
|`amount`|`uint256`|Amount to distribute|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`nextIndex`|`uint256`|Next available index in arrays|


### _distributeToGPs

Distribute amount to GPs


```solidity
function _distributeToGPs(
    address[] memory recipients,
    uint256[] memory amounts,
    uint8[] memory distributionTypes,
    uint256 startIndex,
    address[] memory gps,
    uint256 amount,
    uint8 distType
) internal pure returns (uint256 nextIndex);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`recipients`|`address[]`|Recipients array|
|`amounts`|`uint256[]`|Amounts array|
|`distributionTypes`|`uint8[]`|Distribution types array|
|`startIndex`|`uint256`|Starting index in arrays|
|`gps`|`address[]`|Array of GP addresses|
|`amount`|`uint256`|Amount to distribute|
|`distType`|`uint8`|Distribution type|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`nextIndex`|`uint256`|Next available index in arrays|


## Events
### DistributionCalculated
Emitted when distribution is calculated


```solidity
event DistributionCalculated(uint256 totalAmount, uint256 recipientCount);
```

### DistributionExecuted
Emitted when distribution is executed


```solidity
event DistributionExecuted(address indexed recipient, uint256 amount, uint8 distributionType);
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
event WaterfallModelUpdated(uint8 oldModel, uint8 newModel);
```

### HurdleRateAchieved
Emitted when hurdle rate is achieved


```solidity
event HurdleRateAchieved(uint256 totalReturn, uint256 hurdleRate);
```

### DistributionConfigUpdated
Emitted when distribution configuration is updated


```solidity
event DistributionConfigUpdated(uint8 model, uint256 preferredReturnRate, uint256 carriedInterestSplit);
```

## Errors
### AdvancedDistributionFacet__InvalidAmount

```solidity
error AdvancedDistributionFacet__InvalidAmount();
```

### AdvancedDistributionFacet__InvalidPeriod

```solidity
error AdvancedDistributionFacet__InvalidPeriod();
```

### AdvancedDistributionFacet__ExcessivePreferredReturn

```solidity
error AdvancedDistributionFacet__ExcessivePreferredReturn();
```

### AdvancedDistributionFacet__ExcessiveCarry

```solidity
error AdvancedDistributionFacet__ExcessiveCarry();
```

### AdvancedDistributionFacet__NotGP

```solidity
error AdvancedDistributionFacet__NotGP();
```

### AdvancedDistributionFacet__Unauthorized

```solidity
error AdvancedDistributionFacet__Unauthorized();
```

### AdvancedDistributionFacet__InvalidModel

```solidity
error AdvancedDistributionFacet__InvalidModel();
```

