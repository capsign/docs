# ERC20Collateral
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Trading/Collateral/ERC20Collateral.sol)

**Inherits:**
[BaseCollateral](/src/Trading/Collateral/BaseCollateral.sol/abstract.BaseCollateral.md)

Implements collateral using an ERC20 token.


## State Variables
### collateralToken

```solidity
IERC20 public collateralToken;
```


## Functions
### constructor


```solidity
constructor(address _owner, address _tokenAddress) BaseCollateral(_owner);
```

### deposit

*Deposits ERC20 tokens as collateral.*


```solidity
function deposit(uint256 amount) external override;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount of tokens to deposit.|


### withdraw

*Withdraws ERC20 tokens from collateral.*


```solidity
function withdraw(uint256 amount) external override;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount of tokens to withdraw.|


