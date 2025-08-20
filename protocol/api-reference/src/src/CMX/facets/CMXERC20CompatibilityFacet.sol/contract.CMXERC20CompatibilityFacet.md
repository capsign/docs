# CMXERC20CompatibilityFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/CMX/facets/CMXERC20CompatibilityFacet.sol)

ERC-20 compatibility layer that delegates to lot-based system

*Provides standard ERC-20 interface while using lot-based transfers behind the scenes*


## Functions
### name

Returns the name of the token


```solidity
function name() external view returns (string memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|The token name|


### symbol

Returns the symbol of the token


```solidity
function symbol() external view returns (string memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|The token symbol|


### decimals

Returns the decimals of the token


```solidity
function decimals() external view returns (uint8);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|The token decimals|


### totalSupply

Returns the total supply of the token


```solidity
function totalSupply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total supply|


### balanceOf

Returns the balance of an account (aggregated from all lots)


```solidity
function balanceOf(address account) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The account to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total balance across all lots|


### transfer

Transfer tokens from sender to recipient

*Delegates to CMXTransferFacet using FIFO lot strategy*


```solidity
function transfer(address to, uint256 amount) external returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|The recipient address|
|`amount`|`uint256`|The amount to transfer|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if transfer succeeded|


### transferFrom

Transfer tokens from one account to another (requires allowance)

*Manages allowances and delegates to lot-based transfer*


```solidity
function transferFrom(address from, address to, uint256 amount) external returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The sender address|
|`to`|`address`|The recipient address|
|`amount`|`uint256`|The amount to transfer|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if transfer succeeded|


### allowance

Returns the allowance of spender for owner


```solidity
function allowance(address owner, address spender) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|
|`spender`|`address`|The spender address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The allowance amount|


### approve

Approve spender to spend tokens on behalf of caller


```solidity
function approve(address spender, uint256 amount) external returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`spender`|`address`|The spender address|
|`amount`|`uint256`|The amount to approve|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if approval succeeded|


### increaseAllowance

Increase the allowance for spender


```solidity
function increaseAllowance(address spender, uint256 addedValue) external returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`spender`|`address`|The spender address|
|`addedValue`|`uint256`|The amount to add to allowance|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if increase succeeded|


### decreaseAllowance

Decrease the allowance for spender


```solidity
function decreaseAllowance(address spender, uint256 subtractedValue) external returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`spender`|`address`|The spender address|
|`subtractedValue`|`uint256`|The amount to subtract from allowance|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if decrease succeeded|


### _approve

*Internal approve function*


```solidity
function _approve(address owner, address spender, uint256 amount) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|
|`spender`|`address`|The spender address|
|`amount`|`uint256`|The amount to approve|


### _spendAllowance

*Internal function to spend allowance*


```solidity
function _spendAllowance(address owner, address spender, uint256 amount) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|
|`spender`|`address`|The spender address|
|`amount`|`uint256`|The amount to spend|


## Events
### Transfer

```solidity
event Transfer(address indexed from, address indexed to, uint256 value);
```

### Approval

```solidity
event Approval(address indexed owner, address indexed spender, uint256 value);
```

## Errors
### ERC20_InsufficientAllowance

```solidity
error ERC20_InsufficientAllowance();
```

### ERC20_InvalidAddress

```solidity
error ERC20_InvalidAddress();
```

