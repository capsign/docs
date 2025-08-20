# IDiamondCut
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/facets/cut/IDiamondCut.sol)

**Inherits:**
[IDiamondCutBase](/src/Diamonds/facets/cut/IDiamondCutBase.sol/interface.IDiamondCutBase.md)

Interface of the DiamondCut facet. See [EIP-2535](https://eips.ethereum.org/EIPS/eip-2535).


## Functions
### diamondCut

Add/replace/remove any number of functions and optionally execute
a function with delegatecall.


```solidity
function diamondCut(IDiamond.FacetCut[] calldata facetCuts, address init, bytes calldata initData) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`facetCuts`|`IDiamond.FacetCut[]`|Contains the facet addresses and function selectors.|
|`init`|`address`|The address of the contract or facet to execute initData.|
|`initData`|`bytes`|A function call, including function selector and arguments executed with delegatecall on address init.|


