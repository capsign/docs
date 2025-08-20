# MarginAccount
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Trading/Margin/MarginAccount.sol)

**Inherits:**
[BaseMargin](/src/Trading/Margin/BaseMargin.sol/abstract.BaseMargin.md)

Implements a margin account, allowing users to borrow against collateral to take leveraged positions.


## State Variables
### collateral

```solidity
BaseCollateral public collateral;
```


### borrowedAmount

```solidity
uint256 public borrowedAmount;
```


### collateralFactor

```solidity
uint256 public collateralFactor;
```


## Functions
### constructor


```solidity
constructor(address _owner, address _collateral, uint256 _collateralFactor) BaseMargin(_owner);
```

### openMargin

*Opens a new margin account by depositing initial collateral.*


```solidity
function openMargin(uint256 initialCollateral) external override;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`initialCollateral`|`uint256`|The amount of collateral to deposit initially.|


### closeMargin

*Closes the margin account, ensuring all borrowed funds are repaid.*


```solidity
function closeMargin() external override;
```

### addCollateral

*Adds additional collateral to the margin account.*


```solidity
function addCollateral(uint256 amount) external override;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount of collateral to add.|


### removeCollateral

*Removes collateral from the margin account, ensuring it doesn’t violate the collateral factor.*


```solidity
function removeCollateral(uint256 amount) external override;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount of collateral to remove.|


### openPosition

*Opens a leveraged position by borrowing against collateral.*


```solidity
function openPosition(uint256 size) external override;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`size`|`uint256`|The size of the position to open.|


### closePosition

*Closes a leveraged position by repaying borrowed funds.*


```solidity
function closePosition(uint256 size) external override;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`size`|`uint256`|The size of the position to close.|


### getCollateralBalance

*Retrieves the current collateral balance.*


```solidity
function getCollateralBalance() public pure returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The collateral balance.|


