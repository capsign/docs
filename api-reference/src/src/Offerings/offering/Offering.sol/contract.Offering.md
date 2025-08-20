# Offering
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Offerings/offering/Offering.sol)

**Inherits:**
[Diamond](/src/Diamonds/Diamond.sol/contract.Diamond.md), [AccessManaged](/src/Authorization/AccessManaged.sol/abstract.AccessManaged.md)

This contract replaces the standalone offering contracts with a diamond architecture

*Diamond proxy contract that combines all offering facets*


## Functions
### constructor

*Constructor*


```solidity
constructor(address _authority, bytes memory initData) Diamond(_createInitParams(initData)) AccessManaged(_authority);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_authority`|`address`|The access manager address (IdentityAccessManager)|
|`initData`|`bytes`|Initialization data for the offering|


### _createInitParams

*Create initialization parameters for the diamond*


```solidity
function _createInitParams(bytes memory initData) private returns (Diamond.InitParams memory initParams);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`initData`|`bytes`|Initialization data for the offering|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`initParams`|`Diamond.InitParams`|The initialization parameters|


### receive

*Allow diamond to receive ETH for offering payments*


```solidity
receive() external payable;
```

