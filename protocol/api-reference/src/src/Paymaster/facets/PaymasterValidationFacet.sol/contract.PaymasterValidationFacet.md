# PaymasterValidationFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Paymaster/facets/PaymasterValidationFacet.sol)

**Inherits:**
IPaymaster

Main validation facet for CMX paymaster diamond

*Implements IPaymaster interface for CMX token-based fee payments*


## Functions
### onlyEntryPoint


```solidity
modifier onlyEntryPoint();
```

### whenNotPaused


```solidity
modifier whenNotPaused();
```

### onlyPaymasterManager


```solidity
modifier onlyPaymasterManager();
```

### validatePaymasterUserOp

Validate a user operation and determine if paymaster will pay for it


```solidity
function validatePaymasterUserOp(PackedUserOperation calldata userOp, bytes32 userOpHash, uint256 maxCost)
    external
    override
    onlyEntryPoint
    whenNotPaused
    returns (bytes memory context, uint256 validationData);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`userOp`|`PackedUserOperation`|The user operation to validate|
|`userOpHash`|`bytes32`|Hash of the user operation|
|`maxCost`|`uint256`|Maximum cost of the operation|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`context`|`bytes`|Context data for postOp|
|`validationData`|`uint256`|Validation result (0 = valid)|


### postOp

Post-operation processing to collect CMX fees


```solidity
function postOp(
    IPaymaster.PostOpMode mode,
    bytes calldata context,
    uint256 actualGasCost,
    uint256 actualUserOpFeePerGas
) external onlyEntryPoint;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`mode`|`IPaymaster.PostOpMode`|Post-operation mode|
|`context`|`bytes`|Context data from validatePaymasterUserOp|
|`actualGasCost`|`uint256`|Actual gas cost of the operation|
|`actualUserOpFeePerGas`|`uint256`|The gas price this UserOp pays|


### _extractFactory

Extract factory address from user operation


```solidity
function _extractFactory(PackedUserOperation calldata userOp) internal pure returns (address factory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`userOp`|`PackedUserOperation`|User operation|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`factory`|`address`|Factory address|


### _transferToTreasury

Transfer CMX tokens to protocol treasury


```solidity
function _transferToTreasury(uint256 amount) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|Amount to transfer|


### calculateOperationFee

Calculate total fee for a user operation


```solidity
function calculateOperationFee(PackedUserOperation calldata userOp, address factory)
    external
    view
    returns (uint256 cmxAmount, uint256 ethEquivalent);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`userOp`|`PackedUserOperation`|User operation|
|`factory`|`address`|Target factory contract|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`cmxAmount`|`uint256`|CMX amount required|
|`ethEquivalent`|`uint256`|ETH equivalent amount|


### canPayForOperation

Pre-validate that user can pay for operation


```solidity
function canPayForOperation(address user, address factory, uint256 maxCost)
    external
    view
    returns (bool canPay, uint256 cmxRequired);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User address|
|`factory`|`address`|Target factory contract|
|`maxCost`|`uint256`|Maximum cost in ETH|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`canPay`|`bool`|Whether user can pay|
|`cmxRequired`|`uint256`|CMX amount required|


### _extractGasLimit

Extract gas limit from user operation


```solidity
function _extractGasLimit(PackedUserOperation calldata userOp) internal pure returns (uint256 gasLimit);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`userOp`|`PackedUserOperation`|User operation|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`gasLimit`|`uint256`|Gas limit|


### _extractGasPrice

Extract gas price from user operation


```solidity
function _extractGasPrice(PackedUserOperation calldata userOp) internal pure returns (uint256 gasPrice);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`userOp`|`PackedUserOperation`|User operation|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`gasPrice`|`uint256`|Gas price|


### pause

Emergency pause function

*Only callable by paymaster manager*


```solidity
function pause() external onlyPaymasterManager;
```

### unpause

Emergency unpause function

*Only callable by paymaster manager*


```solidity
function unpause() external onlyPaymasterManager;
```

### isPaused

Check if paymaster is paused


```solidity
function isPaused() external view returns (bool paused);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`paused`|`bool`|Whether the paymaster is paused|


### _checkRateLimits

Check rate limits for a user


```solidity
function _checkRateLimits(address user) internal returns (bool allowed);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`allowed`|`bool`|Whether the operation is allowed|


