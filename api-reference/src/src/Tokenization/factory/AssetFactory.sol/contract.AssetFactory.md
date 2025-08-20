# AssetFactory
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/factory/AssetFactory.sol)

**Inherits:**
[Factory](/src/Diamonds/factory/Factory.sol/contract.Factory.md)

Backward compatibility alias for the generic Factory contract

*This contract provides the same interface as the original AssetFactory*

*but uses the new generic Factory architecture with deployment facets*


## Functions
### constructor

Deploy the AssetFactory diamond using the generic Factory


```solidity
constructor(address _facetRegistry, address _globalAccessManager)
    Factory("AssetFactory", _facetRegistry, _globalAccessManager, "AssetFactory");
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_facetRegistry`|`address`|Address of the FacetRegistry|
|`_globalAccessManager`|`address`|The address of the GlobalAccessManager|


