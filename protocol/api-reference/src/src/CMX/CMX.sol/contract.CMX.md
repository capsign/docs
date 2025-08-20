# CMX
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/CMX/CMX.sol)

**Inherits:**
[Diamond](/src/Diamonds/Diamond.sol/contract.Diamond.md)

CMX Token Diamond - Modular ERC20 token with LayerZero OFT capabilities

*Diamond proxy contract for CMX token with upgradeable facets*


## Functions
### constructor

Deploy the CMX Diamond


```solidity
constructor(Diamond.InitParams memory initParams) Diamond(initParams);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`initParams`|`Diamond.InitParams`|Initial diamond configuration including facets and initialization|


### initializeCMX

Initialize CMX Diamond with token parameters

*This function should be called through diamondCut with proper initialization*


```solidity
function initializeCMX(
    string memory _name,
    string memory _symbol,
    uint8 _decimals,
    address _lzEndpoint,
    address _owner,
    bool _isSourceChain,
    uint256 _initialSupply
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_name`|`string`|Token name|
|`_symbol`|`string`|Token symbol|
|`_decimals`|`uint8`|Token decimals|
|`_lzEndpoint`|`address`|LayerZero endpoint address|
|`_owner`|`address`|Initial owner|
|`_isSourceChain`|`bool`|Whether this chain is a source chain|
|`_initialSupply`|`uint256`|Initial supply to mint (only on source chains)|


### name

Get the CMX token name

*Delegates to CMXAssetCoreFacet*


```solidity
function name() external view returns (string memory);
```

### symbol

Get the CMX token symbol

*Delegates to CMXAssetCoreFacet*


```solidity
function symbol() external view returns (string memory);
```

### decimals

Get the CMX token decimals

*Delegates to CMXAssetCoreFacet*


```solidity
function decimals() external view returns (uint8);
```

### totalSupply

Get the total supply

*Delegates to CMXAssetCoreFacet*


```solidity
function totalSupply() external view returns (uint256);
```

### initialized

Check if CMX is initialized

*Direct storage access for efficiency*


```solidity
function initialized() external view returns (bool);
```

### owner

Get the current owner

*Direct storage access for efficiency*


```solidity
function owner() external view returns (address);
```

### isSourceChain

Check if this is a source chain

*Direct storage access for efficiency*


```solidity
function isSourceChain() external view returns (bool);
```

### getMaxSupply

Get the maximum supply

*Direct access to constant*


```solidity
function getMaxSupply() external pure returns (uint256);
```

### receive

Fallback function to handle ETH transfers

*Rejects direct ETH transfers since CMX doesn't need them*


```solidity
receive() external payable;
```

