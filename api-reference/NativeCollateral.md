# NativeCollateral
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Trading/Collateral/NativeCollateral.sol)

**Inherits:**
[BaseCollateral](/src/Trading/Collateral/BaseCollateral.sol/abstract.BaseCollateral.md)

Implements collateral using native ETH.


## Functions
### constructor


```solidity
constructor(address _owner) BaseCollateral(_owner);
```

### deposit

*Deposits ETH as collateral. Payable to accept ETH.*


```solidity
function deposit() external payable;
```

### deposit


```solidity
function deposit(uint256) external pure override;
```

### withdraw

*Withdraws ETH from collateral.*


```solidity
function withdraw(uint256 amount) external override;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount of ETH to withdraw (in wei).|


### receive

*Fallback function to accept ETH.*


```solidity
receive() external payable;
```

