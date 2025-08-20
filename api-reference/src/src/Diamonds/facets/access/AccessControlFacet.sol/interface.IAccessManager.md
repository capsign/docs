# IAccessManager
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/facets/access/AccessControlFacet.sol)


## Functions
### hasRole


```solidity
function hasRole(uint64 roleId, address account) external view returns (bool);
```

### canCall


```solidity
function canCall(address caller, address target, bytes4 selector) external view returns (bool);
```

