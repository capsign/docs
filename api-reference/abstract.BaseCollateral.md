# BaseCollateral
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Trading/Collateral/BaseCollateral.sol)

Abstract contract defining the basic interface for collateral assets.


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

### deposit

*Deposits collateral into the contract.*


```solidity
function deposit(uint256 amount) external virtual;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount of collateral to deposit.|


### withdraw

*Withdraws collateral from the contract.*


```solidity
function withdraw(uint256 amount) external virtual;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount of collateral to withdraw.|


## Events
### CollateralDeposited

```solidity
event CollateralDeposited(address indexed depositor, uint256 amount);
```

### CollateralWithdrawn

```solidity
event CollateralWithdrawn(address indexed withdrawer, uint256 amount);
```

