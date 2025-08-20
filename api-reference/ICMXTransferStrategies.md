# ICMXTransferStrategies
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/CMX/interfaces/ICMXTransferStrategies.sol)

Interface for CMX transfer strategy management

*Defines the available transfer strategies for optimizing tax efficiency*


## Functions
### setTransferStrategy

Set the transfer strategy for ERC-20 transfers

*Only owner can change the transfer strategy*


```solidity
function setTransferStrategy(CMXStorage.TransferStrategy strategy) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`strategy`|`CMXStorage.TransferStrategy`|The new transfer strategy to use|


### getTransferStrategy

Get the current transfer strategy


```solidity
function getTransferStrategy() external view returns (CMXStorage.TransferStrategy);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`CMXStorage.TransferStrategy`|The current transfer strategy|


## Events
### TransferStrategyChanged
*Emitted when the transfer strategy is changed*


```solidity
event TransferStrategyChanged(
    CMXStorage.TransferStrategy indexed oldStrategy, CMXStorage.TransferStrategy indexed newStrategy
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oldStrategy`|`CMXStorage.TransferStrategy`|The previous transfer strategy|
|`newStrategy`|`CMXStorage.TransferStrategy`|The new transfer strategy|

