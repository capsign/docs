# IDiamondLoupe
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/facets/loupe/IDiamondLoupe.sol)

**Inherits:**
[IDiamondLoupeBase](/src/Diamonds/facets/loupe/IDiamondLoupe.sol/interface.IDiamondLoupeBase.md), IERC165

A loupe is a small magnifying glass used to look at diamonds.
See [EIP-2535](https://eips.ethereum.org/EIPS/eip-2535).


## Functions
### facets

Gets all facet addresses and the selectors of supported functions.


```solidity
function facets() external view returns (Facet[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`Facet[]`|facetInfo An array of Facet structs.|


### facetFunctionSelectors

Gets all the function selectors supported by a specific facet.


```solidity
function facetFunctionSelectors(address facet) external view returns (bytes4[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`facet`|`address`|The facet address.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes4[]`|selectors An array of function selectors.|


### facetAddresses

Get all the facet addresses used by a diamond.


```solidity
function facetAddresses() external view returns (address[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|facets The facet addresses.|


### facetAddress

Gets the facet that supports the given selector.

*If facet is not found return address(0).*


```solidity
function facetAddress(bytes4 selector) external view returns (address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`selector`|`bytes4`|The function selector.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|facetAddress The facet address.|


