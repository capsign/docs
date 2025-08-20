# DiamondLoupeBase
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/facets/loupe/DiamondLoupeBase.sol)

**Inherits:**
[IDiamondLoupeBase](/src/Diamonds/facets/loupe/IDiamondLoupe.sol/interface.IDiamondLoupeBase.md)


## Functions
### _facetSelectors


```solidity
function _facetSelectors(address facet) internal view returns (bytes4[] memory selectors);
```

### _facetAddresses


```solidity
function _facetAddresses() internal view returns (address[] memory);
```

### _facetAddress


```solidity
function _facetAddress(bytes4 selector) internal view returns (address);
```

### _facets


```solidity
function _facets() internal view returns (Facet[] memory facets);
```

### _supportsInterface


```solidity
function _supportsInterface(bytes4 interfaceId) internal view returns (bool);
```

### _addInterface


```solidity
function _addInterface(bytes4 interfaceId) internal;
```

### _removeInterface


```solidity
function _removeInterface(bytes4 interfaceId) internal;
```

