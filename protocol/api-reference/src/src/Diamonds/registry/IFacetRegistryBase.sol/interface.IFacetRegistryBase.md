# IFacetRegistryBase
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/registry/IFacetRegistryBase.sol)


## Events
### FacetRegistered
Emitted when a facet is registered.


```solidity
event FacetRegistered(address indexed facet, bytes4[] selectors);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`facet`|`address`|Address of the registered facet.|
|`selectors`|`bytes4[]`|Function selectors of the registered facet.|

### FacetUnregistered
Emitted when a facet is unregistered.


```solidity
event FacetUnregistered(address indexed facet);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`facet`|`address`|Address of the unregistered facet.|

## Errors
### FacetRegistry_FacetAlreadyRegistered
Reverts when facet is already registered.


```solidity
error FacetRegistry_FacetAlreadyRegistered();
```

### FacetRegistry_FacetAddressZero
Reverts when facet address is zero.


```solidity
error FacetRegistry_FacetAddressZero();
```

### FacetRegistry_FacetMustHaveSelectors
Reverts when facet does not have any selectors.


```solidity
error FacetRegistry_FacetMustHaveSelectors();
```

### FacetRegistry_FacetNotContract
Reverts when facet is not a contract.


```solidity
error FacetRegistry_FacetNotContract();
```

### FacetRegistry_FacetNotRegistered
Reverts when facet is not registered.


```solidity
error FacetRegistry_FacetNotRegistered();
```

