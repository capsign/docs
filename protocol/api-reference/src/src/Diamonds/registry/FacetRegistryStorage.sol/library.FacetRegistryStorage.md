# FacetRegistryStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/registry/FacetRegistryStorage.sol)


## State Variables
### FACET_REGISTRY_STORAGE_POSITION

```solidity
bytes32 public constant FACET_REGISTRY_STORAGE_POSITION = keccak256("facet.registry.storage");
```


## Functions
### layout


```solidity
function layout() internal pure returns (Layout storage l);
```

## Structs
### Layout

```solidity
struct Layout {
    EnumerableSet.AddressSet facets;
    mapping(address facet => EnumerableSet.Bytes32Set selectors) facetSelectors;
}
```

