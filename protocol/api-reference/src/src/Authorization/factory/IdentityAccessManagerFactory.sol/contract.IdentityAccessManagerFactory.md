# IdentityAccessManagerFactory
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Authorization/factory/IdentityAccessManagerFactory.sol)

**Inherits:**
[Factory](/src/Diamonds/factory/Factory.sol/contract.Factory.md)

Factory for creating IdentityAccessManager diamonds using the Factory pattern

*This contract provides the same interface as the original IdentityAccessManagerFactory*

*but uses the new generic Factory architecture with deployment facets*


## Functions
### constructor

Deploy the IdentityAccessManagerFactory diamond using the generic Factory


```solidity
constructor(address _facetRegistry, address _globalAccessManager)
    Factory("IdentityAccessManagerFactory", _facetRegistry, _globalAccessManager, "IdentityAccessManagerFactory");
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_facetRegistry`|`address`|Address of the FacetRegistry|
|`_globalAccessManager`|`address`|The address of the GlobalAccessManager|


