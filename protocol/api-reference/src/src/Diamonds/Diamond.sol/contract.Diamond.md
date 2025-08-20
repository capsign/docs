# Diamond
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/Diamond.sol)

**Inherits:**
[IDiamond](/src/Diamonds/IDiamond.sol/interface.IDiamond.md), Proxy, [DiamondCutBase](/src/Diamonds/facets/cut/DiamondCutBase.sol/abstract.DiamondCutBase.md), [DiamondLoupeBase](/src/Diamonds/facets/loupe/DiamondLoupeBase.sol/abstract.DiamondLoupeBase.md), Initializable

Main diamond proxy contract implementing the EIP-2535 Diamond Standard

*This contract serves as the main proxy that delegates calls to different facets.
It combines the functionality of diamond cutting (adding/removing/replacing facets),
diamond loupe (introspection), and proxy delegation.
Key Features:
- Modular architecture allowing unlimited contract size through facets
- Upgradeable functionality via diamond cuts
- Introspection capabilities via diamond loupe
- Gas-efficient function delegation*


## Functions
### constructor

Diamond constructor that initializes the diamond with base facets

*Uses initializer modifier to prevent re-initialization*


```solidity
constructor(InitParams memory initDiamondCut) initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`initDiamondCut`|`InitParams`|Initial diamond configuration including base facets|


### _implementation

Internal function to get the implementation address for a function call

*Overrides Proxy._implementation() to use diamond storage for function routing*

**Note:**
throws: Diamond_UnsupportedFunction if no facet implements the function


```solidity
function _implementation() internal view override returns (address facet);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`facet`|`address`|The address of the facet that implements the called function|


## Structs
### InitParams
Initialization parameters for diamond deployment


```solidity
struct InitParams {
    FacetCut[] baseFacets;
    address init;
    bytes initData;
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`baseFacets`|`FacetCut[]`|Array of initial facets to add during deployment|
|`init`|`address`|Address of initialization contract (can be zero)|
|`initData`|`bytes`|Calldata for initialization function (can be empty)|

