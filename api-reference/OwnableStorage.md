# OwnableStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/facets/ownable/OwnableStorage.sol)


## State Variables
### OWNABLE_STORAGE_SLOT

```solidity
bytes32 internal constant OWNABLE_STORAGE_SLOT = keccak256("ownable.storage");
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
    address owner;
}
```

