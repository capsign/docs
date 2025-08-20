# OwnableBase
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/facets/ownable/OwnableBase.sol)

**Inherits:**
[IOwnableBase](/src/Diamonds/facets/ownable/IOwnable.sol/interface.IOwnableBase.md)


## Functions
### onlyOwner


```solidity
modifier onlyOwner();
```

### _checkOwner


```solidity
function _checkOwner() internal view;
```

### _owner


```solidity
function _owner() internal view returns (address);
```

### _transferOwnership


```solidity
function _transferOwnership(address newOwner) internal;
```

### _renounceOwnership


```solidity
function _renounceOwnership() internal;
```

