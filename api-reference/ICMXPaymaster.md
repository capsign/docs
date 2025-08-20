# ICMXPaymaster
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Paymaster/interfaces/ICMXPaymaster.sol)

**Inherits:**
IPaymaster

Interface for CMX token-based paymaster diamond

*Extends IPaymaster to support CMX token fee payments with configurable exchange rates*


## Functions
### depositCMX

Deposit CMX tokens to the paymaster for fee payments


```solidity
function depositCMX(uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|Amount of CMX tokens to deposit|


### withdrawCMX

Withdraw CMX tokens from the paymaster


```solidity
function withdrawCMX(uint256 amount, address recipient) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|Amount of CMX tokens to withdraw|
|`recipient`|`address`|Address to receive the tokens|


### getCMXBalance

Get CMX token balance of the paymaster


```solidity
function getCMXBalance() external view returns (uint256 balance);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`balance`|`uint256`|Current CMX token balance|


### getUserCMXAllowance

Get user's CMX allowance for the paymaster


```solidity
function getUserCMXAllowance(address user) external view returns (uint256 allowance);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`allowance`|`uint256`|Current CMX allowance|


### updateExchangeRate

Update CMX/ETH exchange rate

*Only callable by authorized rate updaters*


```solidity
function updateExchangeRate(uint256 newRate) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newRate`|`uint256`|New exchange rate (CMX tokens per 1 ETH, scaled by 1e18)|


### getExchangeRate

Get current CMX/ETH exchange rate


```solidity
function getExchangeRate() external view returns (uint256 rate);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`rate`|`uint256`|Current exchange rate (CMX tokens per 1 ETH, scaled by 1e18)|


### calculateCMXAmount

Calculate CMX amount needed for a given ETH amount


```solidity
function calculateCMXAmount(uint256 ethAmount) external view returns (uint256 cmxAmount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ethAmount`|`uint256`|ETH amount in wei|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`cmxAmount`|`uint256`|Required CMX amount|


### calculateETHEquivalent

Calculate ETH equivalent for a given CMX amount


```solidity
function calculateETHEquivalent(uint256 cmxAmount) external view returns (uint256 ethAmount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`cmxAmount`|`uint256`|CMX amount|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`ethAmount`|`uint256`|Equivalent ETH amount in wei|


### addSupportedFactory

Add a factory to the supported list


```solidity
function addSupportedFactory(address factory, uint256 feeMultiplier) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`factory`|`address`|Factory contract address|
|`feeMultiplier`|`uint256`|Fee multiplier for this factory (scaled by 1e18, 1e18 = 1x)|


### removeSupportedFactory

Remove a factory from the supported list


```solidity
function removeSupportedFactory(address factory) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`factory`|`address`|Factory contract address|


### updateFactoryFeeMultiplier

Update fee multiplier for a factory


```solidity
function updateFactoryFeeMultiplier(address factory, uint256 feeMultiplier) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`factory`|`address`|Factory contract address|
|`feeMultiplier`|`uint256`|New fee multiplier (scaled by 1e18)|


### isFactorySupported

Check if a factory is supported


```solidity
function isFactorySupported(address factory) external view returns (bool supported);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`factory`|`address`|Factory contract address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`supported`|`bool`|Whether the factory is supported|


### getFactoryFeeMultiplier

Get fee multiplier for a factory


```solidity
function getFactoryFeeMultiplier(address factory) external view returns (uint256 multiplier);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`factory`|`address`|Factory contract address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`multiplier`|`uint256`|Fee multiplier (scaled by 1e18)|


### getSupportedFactories

Get all supported factories


```solidity
function getSupportedFactories() external view returns (address[] memory factories);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`factories`|`address[]`|Array of supported factory addresses|


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


### setProtocolTreasury

Set protocol treasury address


```solidity
function setProtocolTreasury(address treasury) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`treasury`|`address`|New treasury address|


### getProtocolTreasury

Get protocol treasury address


```solidity
function getProtocolTreasury() external view returns (address treasury);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`treasury`|`address`|Current treasury address|


### transferFeesToTreasury

Transfer collected fees to protocol treasury


```solidity
function transferFeesToTreasury(uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|Amount of CMX tokens to transfer|


### getPaymasterStats

Get paymaster statistics


```solidity
function getPaymasterStats()
    external
    view
    returns (uint256 totalOperationsSponsored, uint256 totalCMXUsed, uint256 totalETHEquivalent);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalOperationsSponsored`|`uint256`|Total operations sponsored|
|`totalCMXUsed`|`uint256`|Total CMX tokens used for fees|
|`totalETHEquivalent`|`uint256`|Total ETH equivalent value|


### getUserStats

Get user-specific statistics


```solidity
function getUserStats(address user)
    external
    view
    returns (uint256 operationsSponsored, uint256 cmxUsed, uint256 ethEquivalent);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`operationsSponsored`|`uint256`|Operations sponsored for this user|
|`cmxUsed`|`uint256`|CMX tokens used by this user|
|`ethEquivalent`|`uint256`|ETH equivalent value for this user|


## Events
### CMXFeePayment
Emitted when CMX tokens are used to pay for gas


```solidity
event CMXFeePayment(
    address indexed user, address indexed factory, uint256 cmxAmount, uint256 ethEquivalent, uint256 exchangeRate
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The user whose operation was sponsored|
|`factory`|`address`|The factory contract that received the fee|
|`cmxAmount`|`uint256`|Amount of CMX tokens used|
|`ethEquivalent`|`uint256`|Equivalent ETH value|
|`exchangeRate`|`uint256`|CMX/ETH exchange rate used|

### ExchangeRateUpdated
Emitted when exchange rate is updated


```solidity
event ExchangeRateUpdated(uint256 oldRate, uint256 newRate, address indexed updatedBy);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oldRate`|`uint256`|Previous CMX/ETH exchange rate|
|`newRate`|`uint256`|New CMX/ETH exchange rate|
|`updatedBy`|`address`|Address that updated the rate|

### FactoryAdded
Emitted when a factory is added to supported list


```solidity
event FactoryAdded(address indexed factory, uint256 feeMultiplier);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`factory`|`address`|Factory contract address|
|`feeMultiplier`|`uint256`|Fee multiplier for this factory|

### FactoryRemoved
Emitted when a factory is removed from supported list


```solidity
event FactoryRemoved(address indexed factory);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`factory`|`address`|Factory contract address|

### CMXDeposited
Emitted when CMX tokens are deposited to the paymaster


```solidity
event CMXDeposited(address indexed depositor, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`depositor`|`address`|Address that deposited tokens|
|`amount`|`uint256`|Amount of CMX tokens deposited|

### CMXWithdrawn
Emitted when CMX tokens are withdrawn from the paymaster


```solidity
event CMXWithdrawn(address indexed recipient, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`recipient`|`address`|Address that received tokens|
|`amount`|`uint256`|Amount of CMX tokens withdrawn|

## Errors
### InsufficientCMXBalance

```solidity
error InsufficientCMXBalance(uint256 required, uint256 available);
```

### UnsupportedFactory

```solidity
error UnsupportedFactory(address factory);
```

### InvalidExchangeRate

```solidity
error InvalidExchangeRate(uint256 rate);
```

### InvalidFeeMultiplier

```solidity
error InvalidFeeMultiplier(uint256 multiplier);
```

### PaymasterDepositFailed

```solidity
error PaymasterDepositFailed(uint256 amount);
```

### PaymasterWithdrawalFailed

```solidity
error PaymasterWithdrawalFailed(uint256 amount);
```

### UnauthorizedCaller

```solidity
error UnauthorizedCaller(address caller);
```

