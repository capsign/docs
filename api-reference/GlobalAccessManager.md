# GlobalAccessManager
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Authorization/gam/GlobalAccessManager.sol)

**Inherits:**
[Diamond](/src/Diamonds/Diamond.sol/contract.Diamond.md), AccessManager

This contract replaces the standalone GlobalAccessManager with a diamond architecture

*Diamond proxy contract that combines AccessManager with protocol-specific functionality*


## Functions
### constructor

*Constructor*


```solidity
constructor(address _admin) Diamond(_createInitParams(_admin)) AccessManager(_admin);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_admin`|`address`|The initial admin address|


### receive

*Allow diamond to receive ETH if needed*


```solidity
receive() external payable;
```

### _createInitParams

*Create initialization parameters for the diamond*


```solidity
function _createInitParams(address _admin) private returns (Diamond.InitParams memory initParams);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_admin`|`address`|The initial admin address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`initParams`|`Diamond.InitParams`|The initialization parameters|


