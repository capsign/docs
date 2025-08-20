# OfferingFactory
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Offerings/factory/OfferingFactory.sol)

**Inherits:**
[Factory](/src/Diamonds/factory/Factory.sol/contract.Factory.md)

Backward compatibility alias for the generic Factory contract

*This contract provides the same interface as the original OfferingFactory*

*but uses the new generic Factory architecture with deployment facets*


## Functions
### constructor

Deploy the OfferingFactory diamond using the generic Factory


```solidity
constructor(address _facetRegistry, address _globalAccessManager)
    Factory("OfferingFactory", _facetRegistry, _globalAccessManager, "OfferingFactory");
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_facetRegistry`|`address`|Address of the FacetRegistry|
|`_globalAccessManager`|`address`|The address of the GlobalAccessManager|


