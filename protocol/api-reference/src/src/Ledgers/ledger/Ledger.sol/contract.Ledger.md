# Ledger
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Ledgers/ledger/Ledger.sol)

**Inherits:**
[Diamond](/src/Diamonds/Diamond.sol/contract.Diamond.md)

A diamond-based ledger contract representing an organization's complete chart of accounts

*Implements financial accounting with standard account numbering and sections using diamond pattern*


## Functions
### constructor

Constructor for the Ledger diamond


```solidity
constructor(address _owner, uint8 _decimals, string memory _currencyCode)
    Diamond(_createInitParams(_owner, _decimals, _currencyCode));
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_owner`|`address`|The owner of this ledger|
|`_decimals`|`uint8`|The number of decimal places for amounts|
|`_currencyCode`|`string`|The ISO currency code for this ledger|


### _createInitParams

Create initialization parameters for the diamond


```solidity
function _createInitParams(address _owner, uint8 _decimals, string memory _currencyCode)
    internal
    returns (Diamond.InitParams memory initParams);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_owner`|`address`|The owner of this ledger|
|`_decimals`|`uint8`|The number of decimal places for amounts|
|`_currencyCode`|`string`|The ISO currency code for this ledger|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`initParams`|`Diamond.InitParams`|The initialization parameters|


