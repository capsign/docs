# GovernanceFactory
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Governance/factory/GovernanceFactory.sol)

**Inherits:**
[Factory](/src/Diamonds/factory/Factory.sol/contract.Factory.md)

Factory for deploying governance instances using the modern Factory pattern

*Creates TokenholderGovernance and BoardGovernance diamonds with tiered configuration*

*Uses FacetRegistry for template management and FactoryBase for deployment*


## Functions
### constructor

Deploy the GovernanceFactory diamond using the generic Factory


```solidity
constructor(address _facetRegistry, address _globalAccessManager)
    Factory("GovernanceFactory", _facetRegistry, _globalAccessManager, "GovernanceFactory");
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_facetRegistry`|`address`|Address of the FacetRegistry|
|`_globalAccessManager`|`address`|The address of the GlobalAccessManager|


