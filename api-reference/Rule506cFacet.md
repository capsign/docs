# Rule506cFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Offerings/offering/facets/Rule506cFacet.sol)

Implements 506(c) specific compliance including:
- General solicitation allowed
- ALL investors must be accredited
- No limit on number of investors
- Verification of accreditation required

*Specialized facet for Rule 506(c) private placements*

*Uses ComplianceFacet and ExemptionLimitFacet for shared functionality*


## Functions
### initializeRule506c

*Initialize Rule 506(c) specific settings*


```solidity
function initializeRule506c(bool requiresVerification, uint256 exemptionLimit) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requiresVerification`|`bool`|Whether third-party verification of accreditation is required|
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


### validateRule506cInvestment

*Validate investment for Rule 506(c) compliance*


```solidity
function validateRule506cInvestment(address investor, uint256 amount) external view returns (bool valid);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`amount`|`uint256`|The investment amount|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`valid`|`bool`|Whether the investment is valid under 506(c) rules|


### enforceRule506cCompliance

*Enforce Rule 506(c) compliance and exemption limits*


```solidity
function enforceRule506cCompliance(address investor, uint256 amount) external view;
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


### getRule506cConfig

*Get Rule 506(c) specific configuration*


```solidity
function getRule506cConfig()
    external
    view
    returns (bool allowsSolicitation, bool requiresAccreditation, bool hasInvestorLimit);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`allowsSolicitation`|`bool`|Whether general solicitation is allowed (always true for 506(c))|
|`requiresAccreditation`|`bool`|Whether all investors must be accredited (always true for 506(c))|
|`hasInvestorLimit`|`bool`|Whether there's a limit on number of investors (always false for 506(c))|


### getAccreditedInvestors

*Get all accredited investors*


```solidity
function getAccreditedInvestors() external view returns (address[] memory accredited);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`accredited`|`address[]`|Array of addresses that are marked as accredited|


### getAccreditedInvestorCount

*Get count of accredited investors*


```solidity
function getAccreditedInvestorCount() external view returns (uint256 count);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`count`|`uint256`|Number of accredited investors|


### verifyAccreditationStatus

*Verify accreditation status using ComplianceFacet*


```solidity
function verifyAccreditationStatus(address investor) external view returns (bool verified);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`verified`|`bool`|Whether accreditation is verified via attestation|


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

### Rule506cOfferingConfigured

```solidity
event Rule506cOfferingConfigured(bool requiresVerification, uint256 exemptionLimit);
```

### AccreditationVerificationRequired

```solidity
event AccreditationVerificationRequired(address indexed investor);
```

