# Rule506bFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Offerings/offering/facets/Rule506bFacet.sol)

Implements 506(b) specific compliance including:
- No general solicitation
- Maximum 35 non-accredited investors
- Whitelist requirements
- Sophisticated investor requirements for non-accredited

*Specialized facet for Rule 506(b) private placements*

*Uses ComplianceFacet and ExemptionLimitFacet for shared functionality*


## Functions
### initializeRule506b

*Initialize Rule 506(b) specific settings*


```solidity
function initializeRule506b(uint256 maxNonAccredited, bool requireSophistication, uint256 exemptionLimit) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`maxNonAccredited`|`uint256`|Maximum number of non-accredited investors (typically 35)|
|`requireSophistication`|`bool`|Whether to require sophistication for non-accredited investors|
|`exemptionLimit`|`uint256`|12-month rolling exemption limit (0 for unlimited)|


### setInvestorAccreditation

*Set accreditation status for an investor*


```solidity
function setInvestorAccreditation(address investor, bool isAccredited) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`isAccredited`|`bool`|Whether the investor is accredited|


### batchSetInvestorAccreditation

*Batch set accreditation status for multiple investors*


```solidity
function batchSetInvestorAccreditation(address[] calldata investors, bool[] calldata accreditationStatus) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investors`|`address[]`|Array of investor addresses|
|`accreditationStatus`|`bool[]`|Array of accreditation statuses|


### validateRule506bInvestment

*Validate investment for Rule 506(b) compliance*


```solidity
function validateRule506bInvestment(address investor, uint256 amount) external view returns (bool valid);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`amount`|`uint256`|The investment amount|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`valid`|`bool`|Whether the investment is valid under 506(b) rules|


### enforceRule506bCompliance

*Enforce Rule 506(b) compliance and exemption limits*


```solidity
function enforceRule506bCompliance(address investor, uint256 amount) external view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`amount`|`uint256`|The investment amount|


### isAccreditedInvestor

*Check if an investor is accredited*


```solidity
function isAccreditedInvestor(address investor) external view returns (bool isAccredited);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`isAccredited`|`bool`|Whether the investor is accredited|


### getNonAccreditedInvestorCount

*Get the current count of non-accredited investors*


```solidity
function getNonAccreditedInvestorCount() external view returns (uint256 count);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`count`|`uint256`|Number of non-accredited investors|


### getRule506bConfig

*Get Rule 506(b) specific configuration*


```solidity
function getRule506bConfig()
    external
    view
    returns (uint256 maxInvestors, uint256 currentNonAccredited, bool allowsSolicitation);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`maxInvestors`|`uint256`|Maximum number of non-accredited investors allowed|
|`currentNonAccredited`|`uint256`|Current number of non-accredited investors|
|`allowsSolicitation`|`bool`|Whether general solicitation is allowed (always false for 506(b))|


### setNonAccreditedInvestorLimit

*Set investor limit for non-accredited investors*


```solidity
function setNonAccreditedInvestorLimit(uint256 newLimit) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newLimit`|`uint256`|New maximum number of non-accredited investors|


### getAccreditedInvestors

*Get all accredited investors*


```solidity
function getAccreditedInvestors() external view returns (address[] memory accredited);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`accredited`|`address[]`|Array of addresses that are marked as accredited|


### getNonAccreditedInvestors

*Get all non-accredited investors*


```solidity
function getNonAccreditedInvestors() external view returns (address[] memory nonAccredited);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`nonAccredited`|`address[]`|Array of addresses that are marked as non-accredited|


### _requireEntity

*Internal function to check if caller is the entity*


```solidity
function _requireEntity(OfferingStorage.Layout storage s) internal view;
```

## Events
### InvestorAccreditationStatusSet

```solidity
event InvestorAccreditationStatusSet(address indexed investor, bool isAccredited);
```

### Rule506bOfferingConfigured

```solidity
event Rule506bOfferingConfigured(uint256 maxNonAccredited, bool requireSophistication, uint256 exemptionLimit);
```

### NonAccreditedInvestorAdded

```solidity
event NonAccreditedInvestorAdded(address indexed investor);
```

