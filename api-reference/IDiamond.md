# IDiamond
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/IDiamond.sol)

Interface of the Diamond Proxy contract. See [EIP-2535](https://eips.ethereum.org/EIPS/eip-2535).


## Errors
### Diamond_UnsupportedFunction
Thrown when calling a function that was not registered in the diamond.


```solidity
error Diamond_UnsupportedFunction();
```

## Structs
### FacetCut
*Describes a facet to be added, replaced or removed.*


```solidity
struct FacetCut {
    address facet;
    FacetCutAction action;
    bytes4[] selectors;
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`facet`|`address`|Address of the facet, that contains the functions.|
|`action`|`FacetCutAction`|The action to be performed.|
|`selectors`|`bytes4[]`|The function selectors of the facet to be cut.|

### MultiInit
Represents data used in multiDelegateCall.


```solidity
struct MultiInit {
    address init;
    bytes initData;
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`init`|`address`||
|`initData`|`bytes`||

## Enums
### FacetCutAction
Expresses the action of adding, replacing, or removing a facet.


```solidity
enum FacetCutAction {
    Add,
    Replace,
    Remove
}
```

