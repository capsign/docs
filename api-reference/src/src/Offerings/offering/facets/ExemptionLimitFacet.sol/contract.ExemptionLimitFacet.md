# ExemptionLimitFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Offerings/offering/facets/ExemptionLimitFacet.sol)

Handles 12-month rolling exemption limits and monthly tracking
Used by all offering types to avoid code duplication

*Centralized exemption limit tracking for offering diamonds*


## State Variables
### SECONDS_PER_MONTH

```solidity
uint256 public constant SECONDS_PER_MONTH = 30 days;
```


### MONTHS_TO_TRACK

```solidity
uint256 public constant MONTHS_TO_TRACK = 12;
```


## Functions
### setExemptionLimit

*Set the 12-month rolling exemption limit*


```solidity
function setExemptionLimit(uint256 newLimit) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newLimit`|`uint256`|The new exemption limit (0 for unlimited)|


### getExemptionLimit

*Get the current exemption limit*


```solidity
function getExemptionLimit() external view returns (uint256 limit);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`limit`|`uint256`|The current 12-month rolling exemption limit|


### getCurrentMonthId

*Returns the current month ID based on block.timestamp*


```solidity
function getCurrentMonthId() public view returns (uint256);
```

### getTotalIssuedInLast12Months

*Returns the total amount issued in the last 12 months*


```solidity
function getTotalIssuedInLast12Months() public view returns (uint256);
```

### enforceExemptionLimit

*Core function to enforce exemption limits*


```solidity
function enforceExemptionLimit(address investor, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`amount`|`uint256`|The investment amount|


### enforceExemptionLimitWithMessage

*Core function to enforce exemption limits with custom error message*


```solidity
function enforceExemptionLimitWithMessage(address investor, uint256 amount, string calldata errorMessage) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`amount`|`uint256`|The investment amount|
|`errorMessage`|`string`|Custom error message if limit exceeded|


### checkExemptionLimit

*Check if an amount would exceed the exemption limit without updating state*


```solidity
function checkExemptionLimit(uint256 amount) external view returns (bool wouldExceed, uint256 remainingCapacity);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`wouldExceed`|`bool`|Whether the amount would exceed the limit|
|`remainingCapacity`|`uint256`|How much capacity remains|


### getRemainingExemptionCapacity

*Get the remaining exemption capacity*


```solidity
function getRemainingExemptionCapacity() external view returns (uint256 remaining);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`remaining`|`uint256`|Amount remaining under the exemption limit|


### getInvestorAmountIssued

*Get the total amount issued to a specific investor*


```solidity
function getInvestorAmountIssued(address investor) external view returns (uint256 amount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|Total amount issued to the investor|


### getMonthlyTotal

*Get the investment total for a specific month*


```solidity
function getMonthlyTotal(uint256 monthId) external view returns (uint256 total);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`monthId`|`uint256`|The month ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`total`|`uint256`|Total investments for that month|


### getLast12MonthsTotals

*Get the last 12 months' investment totals*


```solidity
function getLast12MonthsTotals() external view returns (uint256[12] memory totals);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totals`|`uint256[12]`|Array of monthly totals for the last 12 months|


### getExemptionStats

*Get exemption statistics*


```solidity
function getExemptionStats()
    external
    view
    returns (uint256 limit, uint256 totalIssued, uint256 remaining, uint256 currentMonth);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`limit`|`uint256`|Current exemption limit|
|`totalIssued`|`uint256`|Total issued in last 12 months|
|`remaining`|`uint256`|Remaining capacity|
|`currentMonth`|`uint256`|Current month ID|


### setInvestorAmountIssued

*Reset investor amount (use with caution, for corrections only)*


```solidity
function setInvestorAmountIssued(address investor, uint256 newAmount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`newAmount`|`uint256`|The new amount for the investor|


### getInvestorAmountsIssued

*Batch get investor amounts*


```solidity
function getInvestorAmountsIssued(address[] calldata investors) external view returns (uint256[] memory amounts);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investors`|`address[]`|Array of investor addresses|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`amounts`|`uint256[]`|Array of amounts issued to each investor|


### _requireEntity

*Internal function to check if caller is the entity*


```solidity
function _requireEntity(OfferingStorage.Layout storage s) internal view;
```

## Events
### ExemptionLimitSet

```solidity
event ExemptionLimitSet(uint256 newLimit);
```

### ExemptionIssuance

```solidity
event ExemptionIssuance(address indexed investor, uint256 amount, uint256 monthId);
```

