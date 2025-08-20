# OfferingDeploymentFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Offerings/factory/facets/OfferingDeploymentFacet.sol)

Handles the creation and deployment of offering diamonds with proper fee handling

*Facet for deploying offering diamonds using unified FacetRegistry*

*Template management moved to unified FacetRegistry*


## Functions
### deployDiamond

Deploy a diamond using FactoryBase pattern


```solidity
function deployDiamond(IDiamond.FacetCut[] memory cuts, bytes memory initData, bytes32 salt)
    external
    returns (address diamondAddress);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`cuts`|`IDiamond.FacetCut[]`|Array of facet cuts to install|
|`initData`|`bytes`|Initialization data for the diamond|
|`salt`|`bytes32`|Salt for deterministic deployment|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`diamondAddress`|`address`|The address of the deployed diamond|


### createOffering

*Create a new offering using a template (no fee)*


```solidity
function createOffering(string calldata templateName, address identityAccessManager, bytes calldata initData)
    external
    returns (address offering);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`templateName`|`string`|The template name to use from FacetRegistry|
|`identityAccessManager`|`address`|The entity's access manager address|
|`initData`|`bytes`|Initialization data for the offering|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`offering`|`address`|The address of the created offering diamond|


### createOfferingWithFee

*Create a new offering using a template with fee payment*


```solidity
function createOfferingWithFee(string calldata templateName, address identityAccessManager, bytes calldata initData)
    external
    payable
    returns (address offering);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`templateName`|`string`|The template name to use from FacetRegistry|
|`identityAccessManager`|`address`|The entity's access manager address|
|`initData`|`bytes`|Initialization data for the offering|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`offering`|`address`|The address of the created offering diamond|


### getAllOfferings

*Get all created offerings*


```solidity
function getAllOfferings() external view returns (address[] memory offerings);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`offerings`|`address[]`|Array of all offering addresses|


### getOfferingsByType

*Get offerings by type*


```solidity
function getOfferingsByType(string calldata offeringType) external view returns (address[] memory offerings);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`offeringType`|`string`|The offering type to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`offerings`|`address[]`|Array of offering addresses of the specified type|


### getOfferingsByEntity

*Get offerings by entity*


```solidity
function getOfferingsByEntity(address entity) external view returns (address[] memory offerings);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|The entity address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`offerings`|`address[]`|Array of offering addresses for the entity|


### getOfferingType

*Get offering type for an offering address*


```solidity
function getOfferingType(address offering) external view returns (string memory offeringType);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`offering`|`address`|The offering address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`offeringType`|`string`|The type of the offering|


### getOfferingEntity

*Get offering entity for an offering address*


```solidity
function getOfferingEntity(address offering) external view returns (address entity);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`offering`|`address`|The offering address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|The entity address|


### getTotalOfferingsCreated

*Get total number of offerings created*


```solidity
function getTotalOfferingsCreated() external view returns (uint256 total);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`total`|`uint256`|Total offerings created|


### getOfferingCountByType

*Get offering count by type*


```solidity
function getOfferingCountByType(string calldata offeringType) external view returns (uint256 count);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`offeringType`|`string`|The offering type|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`count`|`uint256`|Number of offerings of this type|


### getEntityOfferingCount

*Get entity offering count*


```solidity
function getEntityOfferingCount(address entity) external view returns (uint256 count);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|The entity address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`count`|`uint256`|Number of offerings created by this entity|


### _createOfferingInternal

*Internal function to create offering using FacetRegistry*


```solidity
function _createOfferingInternal(
    OfferingFactoryStorage.Layout storage s,
    string calldata templateName,
    address identityAccessManager,
    bytes calldata initData,
    uint256 feeAmount
) internal returns (address offering);
```

### _getCreationFee

*Get creation fee for offering type*


```solidity
function _getCreationFee(OfferingFactoryStorage.Layout storage s, string memory offeringType)
    internal
    view
    returns (uint256 fee);
```

### _handleFeePayment

*Handle fee payment*


```solidity
function _handleFeePayment(OfferingFactoryStorage.Layout storage s, uint256 fee, string memory offeringType) internal;
```

### _requireNotPaused

*Check if factory is not paused*

*Now delegates to AccessControlFacet.paused() for standardized pause state*


```solidity
function _requireNotPaused() internal view;
```

### _requireAuthorized

*Check if caller is authorized (has ENTITY_ROLE)*


```solidity
function _requireAuthorized() internal view;
```

### _getAuthority

*Get the authority address from the diamond*


```solidity
function _getAuthority() internal view returns (address);
```

## Events
### OfferingCreated

```solidity
event OfferingCreated(address indexed offering, address indexed entity, string offeringType, string templateName);
```

### OfferingCreationFeeCollected

```solidity
event OfferingCreationFeeCollected(address indexed entity, uint256 fee, string offeringType);
```

## Errors
### TemplateNotFound

```solidity
error TemplateNotFound();
```

### DeploymentFailed

```solidity
error DeploymentFailed();
```

