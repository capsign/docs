# DiamondCutBase
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/facets/cut/DiamondCutBase.sol)

**Inherits:**
[IDiamondCutBase](/src/Diamonds/facets/cut/IDiamondCutBase.sol/interface.IDiamondCutBase.md)


## Functions
### _diamondCut


```solidity
function _diamondCut(IDiamond.FacetCut[] memory facetCuts, address init, bytes memory initData) internal;
```

### _addFacet


```solidity
function _addFacet(address facet, bytes4[] memory selectors) internal;
```

### _replaceFacet


```solidity
function _replaceFacet(address facet, bytes4[] memory selectors) internal;
```

### _removeFacet


```solidity
function _removeFacet(address facet, bytes4[] memory selectors) internal;
```

### _validateFacetCut


```solidity
function _validateFacetCut(IDiamond.FacetCut memory facetCut) internal view;
```

### _initializeDiamondCut


```solidity
function _initializeDiamondCut(IDiamond.FacetCut[] memory, address init, bytes memory initData) internal;
```

### _multiDelegateCall


```solidity
function _multiDelegateCall(IDiamond.MultiInit[] memory initData) internal;
```

