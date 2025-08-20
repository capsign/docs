# SarStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/storage/SarStorage.sol)

Diamond storage library for Stock Appreciation Rights functionality

*Uses deterministic storage slots to avoid storage collisions with IssuedAsset*


## State Variables
### SAR_STORAGE_POSITION

```solidity
bytes32 constant SAR_STORAGE_POSITION = keccak256("capsign.storage.sar");
```


## Functions
### layout

Get the SAR storage layout


```solidity
function layout() internal pure returns (Layout storage l);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`l`|`Layout`|The storage layout struct|


### initializeSarStorage

Initialize the SAR storage with default values


```solidity
function initializeSarStorage() internal;
```

### createSARTokenInternal

Create a new SAR token


```solidity
function createSARTokenInternal(bytes32 tokenId, address holder, uint96 quantity, uint96 basePrice) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The unique token ID|
|`holder`|`address`|The holder address|
|`quantity`|`uint96`|The number of SAR units|
|`basePrice`|`uint96`|The base price for appreciation calculation|


### invalidateSARTokenInternal

Invalidate a SAR token


```solidity
function invalidateSARTokenInternal(bytes32 tokenId) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID to invalidate|


### getSARTokenInternal

Get SAR token information


```solidity
function getSARTokenInternal(bytes32 tokenId) internal view returns (SARToken memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`SARToken`|The SAR token struct|


### isSARTokenValidInternal

Check if a SAR token is valid


```solidity
function isSARTokenValidInternal(bytes32 tokenId) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if valid|


### getHolderSARsInternal

Get all SAR tokens for a holder


```solidity
function getHolderSARsInternal(address holder) internal view returns (bytes32[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`holder`|`address`|The holder address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32[]`|Array of token IDs|


### getHolderSARCountInternal

Get the number of SAR tokens for a holder


```solidity
function getHolderSARCountInternal(address holder) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`holder`|`address`|The holder address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The count of SAR tokens|


### calculateAppreciationInternal

Calculate appreciation for a SAR token


```solidity
function calculateAppreciationInternal(bytes32 tokenId, uint96 currentPrice) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|
|`currentPrice`|`uint96`|The current price|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The appreciation amount (quantity * (currentPrice - basePrice))|


### generateSARTokenIdInternal

Generate a SAR token ID


```solidity
function generateSARTokenIdInternal(address holder, uint96 quantity, uint96 basePrice)
    internal
    view
    returns (bytes32);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`holder`|`address`|The holder address|
|`quantity`|`uint96`|The quantity|
|`basePrice`|`uint96`|The base price|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|The generated token ID|


### isSarInitialized

Check if SAR is initialized


```solidity
function isSarInitialized() internal view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if initialized|


### updateSARTokenHolderInternal

Update SAR token holder (for transfers)


```solidity
function updateSARTokenHolderInternal(bytes32 tokenId, address newHolder) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|
|`newHolder`|`address`|The new holder address|


### getHolderTotalQuantityInternal

Get total quantity of SARs for a holder


```solidity
function getHolderTotalQuantityInternal(address holder) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`holder`|`address`|The holder address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total quantity|


### getHolderTotalAppreciationInternal

Get total appreciation for a holder at current price


```solidity
function getHolderTotalAppreciationInternal(address holder, uint96 currentPrice) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`holder`|`address`|The holder address|
|`currentPrice`|`uint96`|The current price|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total appreciation|


## Structs
### SARToken

```solidity
struct SARToken {
    bool isValid;
    address holder;
    uint96 quantity;
    uint96 basePrice;
}
```

### Layout

```solidity
struct Layout {
    mapping(bytes32 => SARToken) sarTokens;
    mapping(address => bytes32[]) holderSARs;
    bool sarInitialized;
}
```

