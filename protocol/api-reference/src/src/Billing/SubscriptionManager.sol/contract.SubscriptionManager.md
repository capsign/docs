# SubscriptionManager
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Billing/SubscriptionManager.sol)

**Inherits:**
[Diamond](/src/Diamonds/Diamond.sol/contract.Diamond.md), [AccessManaged](/src/Authorization/AccessManaged.sol/abstract.AccessManaged.md)

Diamond-based subscription management system for the CMX Protocol

*Manages tiered subscriptions with CMX token discounts and stablecoin payments*


## Functions
### constructor

*Constructor*


```solidity
constructor(address _globalAccessManager)
    Diamond(_createInitParams(_globalAccessManager))
    AccessManaged(_globalAccessManager);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_globalAccessManager`|`address`|Address of the GlobalAccessManager|


### _createInitParams

*Create initialization parameters for the diamond*


```solidity
function _createInitParams(address _globalAccessManager) internal returns (Diamond.InitParams memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`Diamond.InitParams`|InitParams The initialization parameters|


### _getDiamondCutSelectors


```solidity
function _getDiamondCutSelectors() internal pure returns (bytes4[] memory);
```

### _getDiamondLoupeSelectors


```solidity
function _getDiamondLoupeSelectors() internal pure returns (bytes4[] memory);
```

### _getSubscriptionCoreSelectors


```solidity
function _getSubscriptionCoreSelectors() internal pure returns (bytes4[] memory);
```

### _getSubscriptionAdminSelectors


```solidity
function _getSubscriptionAdminSelectors() internal pure returns (bytes4[] memory);
```

### _getSubscriptionViewSelectors


```solidity
function _getSubscriptionViewSelectors() internal pure returns (bytes4[] memory);
```

### receive


```solidity
receive() external payable;
```

## Events
### SubscriptionManagerCreated

```solidity
event SubscriptionManagerCreated(address indexed manager);
```

