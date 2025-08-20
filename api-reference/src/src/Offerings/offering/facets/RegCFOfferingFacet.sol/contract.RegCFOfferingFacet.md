# RegCFOfferingFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Offerings/offering/facets/RegCFOfferingFacet.sol)

Implements Reg CF specific compliance including:
- $5M maximum raise in 12 months
- Per-investor investment limits based on income/net worth
- Intermediary requirements

*Specialized facet for Regulation Crowdfunding offerings*

*Uses ExemptionLimitFacet for shared exemption tracking functionality*


## State Variables
### MAX_RAISE_LIMIT

```solidity
uint256 public constant MAX_RAISE_LIMIT = 5_000_000e6;
```


### HIGH_INCOME_THRESHOLD

```solidity
uint256 public constant HIGH_INCOME_THRESHOLD = 125_000e6;
```


### LOW_INCOME_LIMIT

```solidity
uint256 public constant LOW_INCOME_LIMIT = 2_500e6;
```


### HIGH_INCOME_LIMIT

```solidity
uint256 public constant HIGH_INCOME_LIMIT = 12_500e6;
```


## Functions
### initializeRegCF

*Initialize Regulation Crowdfunding specific settings*


```solidity
function initializeRegCF(uint256 maxRaise, address intermediary) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`maxRaise`|`uint256`|Maximum amount to raise (must not exceed $5M)|
|`intermediary`|`address`|Address of the intermediary (portal or broker-dealer)|


### setInvestorLimit

*Set investor-specific investment limit based on income/net worth*


```solidity
function setInvestorLimit(address investor, uint256 annualIncomeOrNetWorth) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`annualIncomeOrNetWorth`|`uint256`|The investor's annual income or net worth|


### batchSetInvestorLimits

*Batch set investor limits*


```solidity
function batchSetInvestorLimits(address[] calldata investors, uint256[] calldata incomeOrNetWorth) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investors`|`address[]`|Array of investor addresses|
|`incomeOrNetWorth`|`uint256[]`|Array of income/net worth values|


### validateRegCFInvestment

*Validate investment for Regulation Crowdfunding compliance*


```solidity
function validateRegCFInvestment(address investor, uint256 amount) external view returns (bool valid);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`amount`|`uint256`|The investment amount|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`valid`|`bool`|Whether the investment is valid under Reg CF rules|


### enforceRegCFCompliance

*Enforce Regulation Crowdfunding compliance and exemption limits*


```solidity
function enforceRegCFCompliance(address investor, uint256 amount) external view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`amount`|`uint256`|The investment amount|


### setIntermediary

*Update the intermediary*


```solidity
function setIntermediary(address newIntermediary) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newIntermediary`|`address`|Address of the new intermediary|


### getIntermediary

*Get the current intermediary*


```solidity
function getIntermediary() external view returns (address intermediary);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`intermediary`|`address`|Address of the current intermediary|


### getInvestorLimit

*Get investor limit*


```solidity
function getInvestorLimit(address investor) external view returns (uint256 limit);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`limit`|`uint256`|The investor's investment limit|


### setExemptionLimit

*Set the exemption limit (must not exceed $5M)*


```solidity
function setExemptionLimit(uint256 newLimit) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newLimit`|`uint256`|The new exemption limit|


### getRegCFConfig

*Get Regulation Crowdfunding configuration*


```solidity
function getRegCFConfig() external view returns (uint256 maxRaise, address intermediary);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`maxRaise`|`uint256`|The maximum raise amount|
|`intermediary`|`address`|Address of the intermediary|


### _requireEntity

*Internal function to check if caller is the entity*


```solidity
function _requireEntity(OfferingStorage.Layout storage s) internal view;
```

## Events
### RegCFOfferingConfigured

```solidity
event RegCFOfferingConfigured(uint256 maxRaise, address intermediary);
```

### InvestorLimitSet

```solidity
event InvestorLimitSet(address indexed investor, uint256 limit);
```

### IntermediaryUpdated

```solidity
event IntermediaryUpdated(address indexed oldIntermediary, address indexed newIntermediary);
```

