# CMX
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/CMX.sol)

**Inherits:**
[Diamond](/src/Diamonds/Diamond.sol/contract.Diamond.md)

Diamond-based omnichain fungible token using LayerZero OFT standard

*Diamond implementation for modular CMX token functionality*


## Functions
### constructor

Initialize CMX diamond


```solidity
constructor(
    Diamond.InitParams memory initParams,
    address _lzEndpoint,
    address _owner,
    bool _isSourceChain,
    uint256 _initialSupply
) Diamond(initParams);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`initParams`|`Diamond.InitParams`|Diamond initialization parameters|
|`_lzEndpoint`|`address`|LayerZero endpoint address|
|`_owner`|`address`|Initial owner of the contract|
|`_isSourceChain`|`bool`|Whether this chain is the source chain|
|`_initialSupply`|`uint256`|Initial supply to mint (only on source chain)|


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


### endpoint

Get LayerZero endpoint


```solidity
function endpoint() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The LayerZero endpoint address|


### owner

Get owner


```solidity
function owner() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The owner address|


### MAX_SUPPLY

Get max supply


```solidity
function MAX_SUPPLY() external pure returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The maximum supply|


### isSourceChain

Check if this is the source chain


```solidity
function isSourceChain() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if source chain|


### initialized

Check if contract is initialized


```solidity
function initialized() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if initialized|


### oftVersion

Get LayerZero OFT version


```solidity
function oftVersion() external pure returns (bytes4 interfaceId, uint64 version);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`interfaceId`|`bytes4`|The interface identifier|
|`version`|`uint64`|The version number|


### receive


```solidity
receive() external payable;
```

