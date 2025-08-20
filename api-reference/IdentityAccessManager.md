# IdentityAccessManager
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Authorization/iam/IdentityAccessManager.sol)

**Inherits:**
[Diamond](/src/Diamonds/Diamond.sol/contract.Diamond.md), AccessManager

This contract supports various legal entity types

*Diamond proxy contract that combines AccessManager with entity-specific functionality*

*Uses pre-deployed facets from FacetRegistry to minimize initcode size*


## Functions
### constructor

*Constructor*


```solidity
constructor(address _entity, string memory _entityName, address _globalAccessManager, address _facetRegistry)
    Diamond(_createInitParams(_entity, _entityName, _globalAccessManager, _facetRegistry))
    AccessManager(_entity);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_entity`|`address`|The entity address|
|`_entityName`|`string`|Human-readable name for the entity|
|`_globalAccessManager`|`address`|The global access manager address|
|`_facetRegistry`|`address`|The facet registry containing pre-deployed facets|


### _createInitParams

*Create initialization parameters for the diamond*


```solidity
function _createInitParams(
    address _entity,
    string memory _entityName,
    address _globalAccessManager,
    address _facetRegistry
) private view returns (Diamond.InitParams memory initParams);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_entity`|`address`|The entity address|
|`_entityName`|`string`|Human-readable name for the entity|
|`_globalAccessManager`|`address`|The global access manager address|
|`_facetRegistry`|`address`|The facet registry containing pre-deployed facets|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`initParams`|`Diamond.InitParams`|The initialization parameters|


### _buildMinimalFacetCuts

*Build minimal facet cuts as fallback if template doesn't exist*


```solidity
function _buildMinimalFacetCuts(FacetRegistry registry) private view returns (IDiamond.FacetCut[] memory cuts);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`registry`|`FacetRegistry`|The facet registry|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`cuts`|`IDiamond.FacetCut[]`|Array of minimal facet cuts|


### receive

*Allow diamond to receive ETH if needed*


```solidity
receive() external payable;
```

