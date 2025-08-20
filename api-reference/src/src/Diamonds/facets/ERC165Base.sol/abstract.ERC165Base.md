# ERC165Base
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/facets/ERC165Base.sol)

**Inherits:**
IERC165

Base contract for ERC165 interface support

*Provides common supportsInterface functionality for Diamond facets*

*Reduces code duplication across CMX Protocol contracts*


## Functions
### supportsInterface

*See [IERC165-supportsInterface](/src/Diamonds/facets/loupe/DiamondLoupeFacet.sol/contract.DiamondLoupeFacet.md#supportsinterface)*


```solidity
function supportsInterface(bytes4 interfaceId) public view virtual override returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`interfaceId`|`bytes4`|The interface identifier to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if the interface is supported|


### _supportsInterface

*Internal function to check interface support*


```solidity
function _supportsInterface(bytes4 interfaceId) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`interfaceId`|`bytes4`|The interface identifier to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if the interface is supported|


### _addInterface

*Add support for an interface*


```solidity
function _addInterface(bytes4 interfaceId) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`interfaceId`|`bytes4`|The interface identifier to add|


### _removeInterface

*Remove support for an interface*


```solidity
function _removeInterface(bytes4 interfaceId) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`interfaceId`|`bytes4`|The interface identifier to remove|


### _initializeERC165

*Initialize common interfaces*

*Should be called in facet initialization functions*


```solidity
function _initializeERC165() internal;
```

