# IDiamondCutBase
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/facets/cut/IDiamondCutBase.sol)


## Events
### DiamondCut
*Emitted when a facet is added, replaced or removed.*


```solidity
event DiamondCut(IDiamond.FacetCut[] facetCuts, address init, bytes initData);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`facetCuts`|`IDiamond.FacetCut[]`|The Facet actions that were performed.|
|`init`|`address`|The address where the initialization was delegated to.|
|`initData`|`bytes`|The data that was passed to the initialization function.|

## Errors
### DiamondCut_SelectorArrayEmpty
Thrown when the facet cut has no selectors.


```solidity
error DiamondCut_SelectorArrayEmpty(address facet);
```

### DiamondCut_FacetIsZeroAddress
Thrown when the facet address is zero address.


```solidity
error DiamondCut_FacetIsZeroAddress();
```

### DiamondCut_FacetIsNotContract
Thrown when the facet address isn't a contract.


```solidity
error DiamondCut_FacetIsNotContract(address facet);
```

### DiamondCut_IncorrectFacetCutAction
Thrown when the facet cut action doesn't exist.


```solidity
error DiamondCut_IncorrectFacetCutAction();
```

### DiamondCut_SelectorIsZero
Thrown when a facet cut selector is zero.


```solidity
error DiamondCut_SelectorIsZero();
```

### DiamondCut_FunctionAlreadyExists
Thrown when a facet cut selector being added already exists.


```solidity
error DiamondCut_FunctionAlreadyExists(bytes4 selector);
```

### DiamondCut_CannotRemoveFromOtherFacet
Thrown when facet cut tries to remove selectors from another facet.


```solidity
error DiamondCut_CannotRemoveFromOtherFacet(address facet, bytes4 selector);
```

### DiamondCut_FunctionFromSameFacet
Thrown when diamond cut tries to replace a facet with itself.


```solidity
error DiamondCut_FunctionFromSameFacet(bytes4 selector);
```

### DiamondCut_NonExistingFunction
Thrown when a diamond cut tries to replace a facet that doesn't exist.


```solidity
error DiamondCut_NonExistingFunction(bytes4 selector);
```

### DiamondCut_ImmutableFacet
Thrown when the diamond cut tries to remove an immutable facet.


```solidity
error DiamondCut_ImmutableFacet();
```

### DiamondCut_InitIsNotContract
Thrown when trying to send init data to an address that isn't a contract.


```solidity
error DiamondCut_InitIsNotContract(address init);
```

