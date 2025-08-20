# RegSOfferingFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Offerings/offering/facets/RegSOfferingFacet.sol)

Implements Reg S specific compliance including:
- Foreign investor verification
- Distribution compliance periods
- Category-specific restrictions

*Specialized facet for Regulation S offshore offerings*

*Uses ComplianceFacet and ExemptionLimitFacet for shared functionality*


## State Variables
### CAT_TWO_COMPLIANCE_PERIOD

```solidity
uint256 public constant CAT_TWO_COMPLIANCE_PERIOD = 40 days;
```


### CAT_THREE_COMPLIANCE_PERIOD

```solidity
uint256 public constant CAT_THREE_COMPLIANCE_PERIOD = 365 days;
```


## Functions
### initializeRegS

*Initialize Regulation S specific settings*


```solidity
function initializeRegS(uint8 category, address attestationRegistry, uint256 exemptionLimit) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`category`|`uint8`|The Regulation S category (1, 2, or 3)|
|`attestationRegistry`|`address`|Address of the attestation registry|
|`exemptionLimit`|`uint256`|12-month rolling exemption limit (0 for unlimited)|


### _setCategory

*Set the Regulation S category and corresponding compliance period*


```solidity
function _setCategory(OfferingStorage.Layout storage s, uint8 category) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`s`|`OfferingStorage.Layout`||
|`category`|`uint8`|The Regulation S category (1, 2, or 3)|


### validateRegSInvestment

*Validate investment for Regulation S compliance*


```solidity
function validateRegSInvestment(address investor, uint256 amount) external view returns (bool valid);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`amount`|`uint256`|The investment amount|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`valid`|`bool`|Whether the investment is valid under Reg S rules|


### enforceRegSCompliance

*Enforce Regulation S compliance and exemption limits*


```solidity
function enforceRegSCompliance(address investor, uint256 amount) external view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`amount`|`uint256`|The investment amount|


### isCompliancePeriodActive

*Check if the distribution compliance period is active*


```solidity
function isCompliancePeriodActive() public view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if the compliance period is active|


### getRegSConfig

*Get Regulation S configuration*


```solidity
function getRegSConfig()
    external
    view
    returns (uint8 category, uint256 complianceStart, uint256 complianceEnd, bool isActive);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`category`|`uint8`|The Reg S category|
|`complianceStart`|`uint256`|Distribution compliance start time|
|`complianceEnd`|`uint256`|Distribution compliance end time|
|`isActive`|`bool`|Whether compliance period is active|


### _requireEntity

*Internal function to check if caller is the entity*


```solidity
function _requireEntity(OfferingStorage.Layout storage s) internal view;
```

## Events
### RegSOfferingConfigured

```solidity
event RegSOfferingConfigured(uint8 category, uint256 complianceStart, uint256 complianceEnd, uint256 exemptionLimit);
```

## Enums
### Category

```solidity
enum Category {
    CategoryOne,
    CategoryTwo,
    CategoryThree
}
```

