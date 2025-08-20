# DiamondCutFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/facets/cut/DiamondCutFacet.sol)

**Inherits:**
[IDiamondCut](/src/Diamonds/facets/cut/IDiamondCut.sol/interface.IDiamondCut.md), [DiamondCutBase](/src/Diamonds/facets/cut/DiamondCutBase.sol/abstract.DiamondCutBase.md), [Facet](/src/Diamonds/facets/Facet.sol/abstract.Facet.md)


## Functions
### DiamondCut_init


```solidity
function DiamondCut_init() external onlyInitializing;
```

### diamondCut

Add/replace/remove any number of functions and optionally execute
a function with delegatecall.


```solidity
function diamondCut(IDiamond.FacetCut[] memory facetCuts, address init, bytes memory initData)
    external
    onlyDiamondOwner
    reinitializer(_getInitializedVersion() + 1);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`facetCuts`|`IDiamond.FacetCut[]`|Contains the facet addresses and function selectors.|
|`init`|`address`|The address of the contract or facet to execute initData.|
|`initData`|`bytes`|A function call, including function selector and arguments executed with delegatecall on address init.|


