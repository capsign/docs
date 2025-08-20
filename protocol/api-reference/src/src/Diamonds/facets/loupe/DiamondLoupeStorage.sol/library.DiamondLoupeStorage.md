# DiamondLoupeStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/facets/loupe/DiamondLoupeStorage.sol)


## State Variables
### DIAMOND_LOUPE_STORAGE

```solidity
bytes32 internal constant DIAMOND_LOUPE_STORAGE = keccak256("diamond.loupe.storage");
```


## Functions
### layout


```solidity
function layout() internal pure returns (Storage storage l);
```

## Structs
### Storage

```solidity
struct Storage {
    mapping(bytes4 interfaceId => bool isSupported) supportedInterfaces;
}
```

