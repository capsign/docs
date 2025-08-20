# BaseMargin
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Trading/Margin/BaseMargin.sol)

Abstract contract defining the core interface for margin accounts.


## State Variables
### owner

```solidity
address public owner;
```


## Functions
### constructor


```solidity
constructor(address _owner);
```

### openMargin

*Opens a new margin account with initial collateral.*


```solidity
function openMargin(uint256 initialCollateral) external virtual;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`initialCollateral`|`uint256`|The amount of collateral to deposit initially.|


### closeMargin

*Closes the margin account, withdrawing remaining collateral.*


```solidity
function closeMargin() external virtual;
```

### addCollateral

*Adds collateral to the margin account.*


```solidity
function addCollateral(uint256 amount) external virtual;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount of collateral to add.|


### removeCollateral

*Removes collateral from the margin account.*


```solidity
function removeCollateral(uint256 amount) external virtual;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount of collateral to remove.|


### openPosition

*Opens a leveraged position.*


```solidity
function openPosition(uint256 size) external virtual;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`size`|`uint256`|The size of the position to open.|


### closePosition

*Closes a leveraged position.*


```solidity
function closePosition(uint256 size) external virtual;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`size`|`uint256`|The size of the position to close.|


## Events
### MarginOpened

```solidity
event MarginOpened(address indexed user, uint256 initialCollateral);
```

### MarginClosed

```solidity
event MarginClosed(address indexed user);
```

### CollateralAdded

```solidity
event CollateralAdded(address indexed user, uint256 amount);
```

### CollateralRemoved

```solidity
event CollateralRemoved(address indexed user, uint256 amount);
```

### PositionOpened

```solidity
event PositionOpened(address indexed user, uint256 size);
```

### PositionClosed

```solidity
event PositionClosed(address indexed user, uint256 size);
```

### MarginCall

```solidity
event MarginCall(address indexed user, uint256 requiredCollateral);
```

