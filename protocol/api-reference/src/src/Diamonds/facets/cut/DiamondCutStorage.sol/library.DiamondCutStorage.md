# DiamondCutStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/facets/cut/DiamondCutStorage.sol)


## State Variables
### DIAMOND_CUT_STORAGE_POSITION

```solidity
bytes32 internal constant DIAMOND_CUT_STORAGE_POSITION = keccak256("diamond.cut.storage");
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
    mapping(bytes4 selector => address facet) selectorToFacet;
    mapping(address facet => EnumerableSet.Bytes32Set selectors) facetSelectors;
}
```

