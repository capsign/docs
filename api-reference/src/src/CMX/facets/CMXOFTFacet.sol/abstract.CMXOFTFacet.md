# CMXOFTFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/CMX/facets/CMXOFTFacet.sol)

**Inherits:**
OFTCore

LayerZero OFT functionality for CMX diamond

*Extends OFTCore to provide cross-chain CMX token functionality*


## Functions
### onlyCMXOwner


```solidity
modifier onlyCMXOwner();
```

### onlyInitialized


```solidity
modifier onlyInitialized();
```

### constructor

Constructor for CMX OFT facet


```solidity
constructor(address _lzEndpoint, address _delegate) OFTCore(18, _lzEndpoint, _delegate);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_lzEndpoint`|`address`|LayerZero endpoint address|
|`_delegate`|`address`|The delegate capable of making OApp configurations|


### owner

Get the owner address


```solidity
function owner() public view override returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The owner address from CMX storage|


### token

Get the token address (this diamond)


```solidity
function token() public view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The token address|


### approvalRequired

Indicates whether approval is required for token transfers


```solidity
function approvalRequired() external pure returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|False since the diamond IS the token|


### sharedDecimals

Get shared decimals for cross-chain compatibility


```solidity
function sharedDecimals() public pure override returns (uint8);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|6 decimals (LayerZero standard for max compatibility)|


### _debit

Internal function to debit tokens for cross-chain transfer


```solidity
function _debit(address _from, uint256 _amountLD, uint256 _minAmountLD, uint32 _dstEid)
    internal
    override
    returns (uint256 amountSentLD, uint256 amountReceivedLD);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_from`|`address`|The sender address|
|`_amountLD`|`uint256`|The amount to debit in local decimals|
|`_minAmountLD`|`uint256`|The minimum amount to debit in local decimals|
|`_dstEid`|`uint32`|The destination endpoint ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`amountSentLD`|`uint256`|The actual amount debited|
|`amountReceivedLD`|`uint256`|The amount that will be received|


### _credit

Internal function to credit tokens for cross-chain transfer


```solidity
function _credit(address _to, uint256 _amountLD, uint32 _srcEid) internal override returns (uint256 amountReceivedLD);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_to`|`address`|The recipient address|
|`_amountLD`|`uint256`|The amount to credit in local decimals|
|`_srcEid`|`uint32`|The source endpoint ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`amountReceivedLD`|`uint256`|The actual amount credited|


### name

Get token name


```solidity
function name() external view returns (string memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|The token name|


### symbol

Get token symbol


```solidity
function symbol() external view returns (string memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|The token symbol|


### decimals

Get token decimals


```solidity
function decimals() external view returns (uint8);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|The token decimals|


### totalSupply

Get total supply


```solidity
function totalSupply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total supply|


### balanceOf

Get balance of account


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
|`<none>`|`uint256`|The account balance|


### allowance

Get allowance


```solidity
function allowance(address tokenOwner, address spender) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenOwner`|`address`|The owner address|
|`spender`|`address`|The spender address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The allowance amount|


### transfer

Transfer tokens


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
|`<none>`|`bool`|True if successful|


### transferFrom

Transfer tokens from one address to another


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
|`<none>`|`bool`|True if successful|


### approve

Approve spender to spend tokens


```solidity
function approve(address spender, uint256 amount) external returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`spender`|`address`|The spender address|
|`amount`|`uint256`|The allowance amount|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if successful|


### _spendAllowance

Internal function to spend allowance


```solidity
function _spendAllowance(address tokenOwner, address spender, uint256 amount) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenOwner`|`address`|The token owner|
|`spender`|`address`|The spender|
|`amount`|`uint256`|The amount to spend|


## Errors
### OnlyOwner

```solidity
error OnlyOwner();
```

### NotInitialized

```solidity
error NotInitialized();
```

