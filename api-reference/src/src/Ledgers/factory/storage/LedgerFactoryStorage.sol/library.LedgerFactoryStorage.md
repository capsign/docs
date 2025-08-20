# LedgerFactoryStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Ledgers/factory/storage/LedgerFactoryStorage.sol)

Storage library for ledger factory configuration using diamond storage pattern

*Provides storage for ledger factory settings like fees, limits, and permissions*


## State Variables
### LEDGER_FACTORY_STORAGE_SLOT

```solidity
bytes32 private constant LEDGER_FACTORY_STORAGE_SLOT = keccak256("capsign.storage.ledger_factory");
```


## Functions
### layout

Get the storage layout for ledger factory configuration


```solidity
function layout() internal pure returns (Layout storage l);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`l`|`Layout`|The storage layout|


### initialize

Initialize ledger factory storage


```solidity
function initialize(address feeRecipient, uint256 creationFee, uint256 maxLedgersPerUser) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`feeRecipient`|`address`|Initial fee recipient|
|`creationFee`|`uint256`|Initial creation fee|
|`maxLedgersPerUser`|`uint256`|Initial max ledgers per user|


### isInitialized

Check if ledger factory is initialized


```solidity
function isInitialized() internal view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if initialized|


### incrementLedgersCreated

Increment total ledgers created counter


```solidity
function incrementLedgersCreated() internal;
```

### getTotalLedgersCreated

Get total ledgers created


```solidity
function getTotalLedgersCreated() internal view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Total number of ledgers created|


### setCurrencyAllowed

Set currency permission


```solidity
function setCurrencyAllowed(string memory currency, bool allowed) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`currency`|`string`|The currency code|
|`allowed`|`bool`|Whether the currency is allowed|


### isCurrencyAllowed

Check if currency is allowed


```solidity
function isCurrencyAllowed(string memory currency) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`currency`|`string`|The currency code|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if allowed|


### setDecimalLimits

Set decimal precision limits


```solidity
function setDecimalLimits(uint8 minDecimals, uint8 maxDecimals) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`minDecimals`|`uint8`|Minimum allowed decimals|
|`maxDecimals`|`uint8`|Maximum allowed decimals|


### getDecimalLimits

Get decimal precision limits


```solidity
function getDecimalLimits() internal view returns (uint8 minDecimals, uint8 maxDecimals);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`minDecimals`|`uint8`|Minimum allowed decimals|
|`maxDecimals`|`uint8`|Maximum allowed decimals|


### isDecimalAllowed

Check if decimal precision is within limits


```solidity
function isDecimalAllowed(uint8 decimals) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`decimals`|`uint8`|The decimal precision to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if within limits|


### setFeeRecipient

Set fee recipient address


```solidity
function setFeeRecipient(address feeRecipient) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`feeRecipient`|`address`|New fee recipient address|


### getFeeRecipient

Get fee recipient address


```solidity
function getFeeRecipient() internal view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|Fee recipient address|


## Structs
### Layout

```solidity
struct Layout {
    uint256 creationFee;
    address feeRecipient;
    uint256 maxLedgersPerUser;
    uint256 maxLedgersTotal;
    mapping(string => bool) allowedCurrencies;
    uint8 minDecimals;
    uint8 maxDecimals;
    uint256 totalLedgersCreated;
    bool initialized;
    uint256[50] __gap;
}
```

