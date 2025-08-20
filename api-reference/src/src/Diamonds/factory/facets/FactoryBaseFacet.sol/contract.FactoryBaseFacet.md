# FactoryBaseFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/factory/facets/FactoryBaseFacet.sol)

**Inherits:**
[FactoryBase](/src/Diamonds/factory/FactoryBase.sol/abstract.FactoryBase.md)

Facet implementation of FactoryBase functionality for diamond factories

*Provides template management and query functions as a diamond facet*


## Functions
### constructor

Constructor for the FactoryBaseFacet


```solidity
constructor(address _facetRegistry, address _globalAccessManager) FactoryBase(_facetRegistry, _globalAccessManager);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_facetRegistry`|`address`|Address of the FacetRegistry|
|`_globalAccessManager`|`address`|Address of the GlobalAccessManager|


### deployDiamond

Deploy a diamond with specific facet cuts


```solidity
function deployDiamond(IDiamond.FacetCut[] memory facetCuts, bytes memory constructorArgs, bytes32 salt)
    external
    returns (address diamond);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`facetCuts`|`IDiamond.FacetCut[]`|Array of facet cuts to install|
|`constructorArgs`|`bytes`|ABI-encoded constructor arguments|
|`salt`|`bytes32`|Salt for CREATE2 deployment|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`diamond`|`address`|Address of the deployed diamond|


