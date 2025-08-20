# IFactoryPaymaster
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/factory/IFactoryPaymaster.sol)

Base interface for factory contracts to integrate with paymasters

*Provides standardized methods for paymaster-based fee collection*


## Functions
### updatePaymasterSupport

Update paymaster support


```solidity
function updatePaymasterSupport(address paymaster, bool supported) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`paymaster`|`address`|Paymaster address|
|`supported`|`bool`|Whether to support this paymaster|


### isPaymasterSupported

Check if a paymaster is supported


```solidity
function isPaymasterSupported(address paymaster) external view returns (bool supported);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`paymaster`|`address`|Paymaster address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`supported`|`bool`|Whether the paymaster is supported|


### getSupportedPaymasters

Get all supported paymasters


```solidity
function getSupportedPaymasters() external view returns (address[] memory paymasters);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`paymasters`|`address[]`|Array of supported paymaster addresses|


### preValidatePayment

Pre-validate payment before operation


```solidity
function preValidatePayment(address user, address paymaster, PackedUserOperation calldata userOp)
    external
    view
    returns (bool validated);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User who will pay|
|`paymaster`|`address`|Paymaster address|
|`userOp`|`PackedUserOperation`|User operation (for gas estimation)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`validated`|`bool`|Whether the payment is pre-validated|


### getBaseFee

Get the base creation fee for factory operations


```solidity
function getBaseFee(string calldata operationType) external view returns (uint256 fee);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`operationType`|`string`|Type of operation|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`fee`|`uint256`|Base fee in ETH (wei)|


### calculateTotalFee

Calculate total fee including any factory-specific multipliers


```solidity
function calculateTotalFee(string calldata operationType, uint256 baseAmount)
    external
    view
    returns (uint256 totalFee);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`operationType`|`string`|Type of operation|
|`baseAmount`|`uint256`|Base amount in ETH|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalFee`|`uint256`|Total fee including multipliers|


## Events
### PaymasterSupportUpdated
Emitted when paymaster support is updated


```solidity
event PaymasterSupportUpdated(address indexed paymaster, bool supported);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`paymaster`|`address`|Paymaster address|
|`supported`|`bool`|Whether the paymaster is supported|

### PaymasterUsed
Emitted when a creation operation uses a paymaster


```solidity
event PaymasterUsed(address indexed user, address indexed paymaster, string operationType);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User who initiated the operation|
|`paymaster`|`address`|Paymaster used|
|`operationType`|`string`|Type of operation performed|

## Errors
### UnsupportedPaymaster

```solidity
error UnsupportedPaymaster(address paymaster);
```

### PaymasterValidationFailed

```solidity
error PaymasterValidationFailed(address paymaster, string reason);
```

