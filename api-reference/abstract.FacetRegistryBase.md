# FacetRegistryBase
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/registry/FacetRegistryBase.sol)

**Inherits:**
[IFacetRegistryBase](/src/Diamonds/registry/IFacetRegistryBase.sol/interface.IFacetRegistryBase.md)


## Functions
### _addFacet


```solidity
function _addFacet(address facet, bytes4[] memory selectors) internal;
```

### _removeFacet


```solidity
function _removeFacet(address facet) internal;
```

### _deployFacet


```solidity
function _deployFacet(bytes32 salt, bytes memory creationCode, bytes4[] memory selectors)
    internal
    returns (address facet);
```

### _computeFacetAddress


```solidity
function _computeFacetAddress(bytes32 salt, bytes memory creationCode) internal view returns (address facet);
```

### _facetSelectors


```solidity
function _facetSelectors(address facet) internal view returns (bytes4[] memory selectors);
```

### _facetAddresses


```solidity
function _facetAddresses() internal view returns (address[] memory facets);
```

