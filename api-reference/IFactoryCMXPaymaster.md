# IFactoryCMXPaymaster
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/factory/IFactoryCMXPaymaster.sol)

**Inherits:**
[IFactoryPaymaster](/src/Diamonds/factory/IFactoryPaymaster.sol/interface.IFactoryPaymaster.md)

Interface for factory contracts to integrate with CMX paymaster

*Extends IFactoryPaymaster with CMX-specific functionality*


## Functions
### updateCMXPaymasterSupport

Update CMX paymaster support


```solidity
function updateCMXPaymasterSupport(address paymaster, bool supported) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`paymaster`|`address`|CMX paymaster address|
|`supported`|`bool`|Whether to support this paymaster|


### isCMXPaymasterSupported

Check if a CMX paymaster is supported


```solidity
function isCMXPaymasterSupported(address paymaster) external view returns (bool supported);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`paymaster`|`address`|CMX paymaster address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`supported`|`bool`|Whether the paymaster is supported|


### getSupportedCMXPaymasters

Get all supported CMX paymasters


```solidity
function getSupportedCMXPaymasters() external view returns (address[] memory paymasters);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`paymasters`|`address[]`|Array of supported paymaster addresses|


### validateCMXPayment

Validate CMX paymaster can handle the fee for an operation


```solidity
function validateCMXPayment(address user, address paymaster, string calldata operationType)
    external
    view
    returns (bool canPay, uint256 cmxRequired, uint256 ethEquivalent);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User who will pay the fee|
|`paymaster`|`address`|CMX paymaster address|
|`operationType`|`string`|Type of operation (for fee calculation)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`canPay`|`bool`|Whether the user can pay via CMX|
|`cmxRequired`|`uint256`|CMX amount required|
|`ethEquivalent`|`uint256`|ETH equivalent amount|


### preValidateCMXPayment

Pre-validate CMX payment before operation


```solidity
function preValidateCMXPayment(address user, address paymaster, PackedUserOperation calldata userOp)
    external
    view
    returns (bool validated);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User who will pay|
|`paymaster`|`address`|CMX paymaster address|
|`userOp`|`PackedUserOperation`|User operation (for gas estimation)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`validated`|`bool`|Whether the payment is pre-validated|


## Events
### CMXPaymasterSupportUpdated
Emitted when CMX paymaster support is updated


```solidity
event CMXPaymasterSupportUpdated(address indexed paymaster, bool supported);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`paymaster`|`address`|CMX paymaster address|
|`supported`|`bool`|Whether the paymaster is supported|

### CMXPaymasterUsed
Emitted when a creation operation uses CMX paymaster


```solidity
event CMXPaymasterUsed(address indexed user, address indexed paymaster, uint256 cmxAmount, uint256 ethEquivalent);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User who initiated the operation|
|`paymaster`|`address`|CMX paymaster used|
|`cmxAmount`|`uint256`|CMX amount that will be charged|
|`ethEquivalent`|`uint256`|ETH equivalent value|

## Errors
### UnsupportedCMXPaymaster

```solidity
error UnsupportedCMXPaymaster(address paymaster);
```

### InsufficientCMXForFee

```solidity
error InsufficientCMXForFee(uint256 required, uint256 available);
```

### CMXPaymasterValidationFailed

```solidity
error CMXPaymasterValidationFailed(address paymaster, string reason);
```

