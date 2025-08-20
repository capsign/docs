# WarrantStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/storage/WarrantStorage.sol)

Diamond storage library for Warrant functionality

*Uses deterministic storage slots to avoid storage collisions with IssuedAsset*


## State Variables
### WARRANT_STORAGE_POSITION

```solidity
bytes32 constant WARRANT_STORAGE_POSITION = keccak256("capsign.storage.warrant");
```


## Functions
### layout

Get the Warrant storage layout


```solidity
function layout() internal pure returns (Layout storage l);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`l`|`Layout`|The storage layout struct|


### initializeWarrantStorage

Initialize the Warrant storage with default values


```solidity
function initializeWarrantStorage() internal;
```

### createWarrantTokenInternal

Create a new warrant token


```solidity
function createWarrantTokenInternal(
    bytes32 tokenId,
    address owner,
    uint96 quantity,
    address paymentCurrency,
    uint96 strikePrice
) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The unique token ID|
|`owner`|`address`|The owner address|
|`quantity`|`uint96`|The number of warrants|
|`paymentCurrency`|`address`|The payment currency|
|`strikePrice`|`uint96`|The strike price|


### invalidateWarrantTokenInternal

Invalidate a warrant token


```solidity
function invalidateWarrantTokenInternal(bytes32 tokenId) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID to invalidate|


### getWarrantTokenInternal

Get warrant token information


```solidity
function getWarrantTokenInternal(bytes32 tokenId) internal view returns (WarrantToken memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`WarrantToken`|The warrant token struct|


### isWarrantTokenValidInternal

Check if a warrant token is valid


```solidity
function isWarrantTokenValidInternal(bytes32 tokenId) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if valid|


### getOwnerWarrantsInternal

Get all warrant tokens for an owner


```solidity
function getOwnerWarrantsInternal(address owner) internal view returns (bytes32[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32[]`|Array of token IDs|


### getOwnerWarrantCountInternal

Get the number of warrant tokens for an owner


```solidity
function getOwnerWarrantCountInternal(address owner) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The count of warrant tokens|


### calculateExerciseValueInternal

Calculate exercise value for a warrant token


```solidity
function calculateExerciseValueInternal(bytes32 tokenId, uint96 currentPrice) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|
|`currentPrice`|`uint96`|The current stock price|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The exercise value (quantity * max(0, currentPrice - strikePrice))|


### calculateExerciseCostInternal

Calculate exercise cost for a warrant token


```solidity
function calculateExerciseCostInternal(bytes32 tokenId) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total cost to exercise (quantity * strikePrice)|


### generateWarrantTokenIdInternal

Generate a warrant token ID


```solidity
function generateWarrantTokenIdInternal(address owner, uint96 quantity, address paymentCurrency, uint96 strikePrice)
    internal
    view
    returns (bytes32);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|
|`quantity`|`uint96`|The quantity|
|`paymentCurrency`|`address`|The payment currency|
|`strikePrice`|`uint96`|The strike price|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|The generated token ID|


### isWarrantInitialized

Check if Warrant is initialized


```solidity
function isWarrantInitialized() internal view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if initialized|


### updateWarrantTokenOwnerInternal

Update warrant token owner (for transfers)


```solidity
function updateWarrantTokenOwnerInternal(bytes32 tokenId, address newOwner) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|
|`newOwner`|`address`|The new owner address|


### getOwnerTotalQuantityInternal

Get total quantity of warrants for an owner


```solidity
function getOwnerTotalQuantityInternal(address owner) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total quantity|


### getOwnerTotalExerciseValueInternal

Get total exercise value for an owner at current price


```solidity
function getOwnerTotalExerciseValueInternal(address owner, uint96 currentPrice) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|
|`currentPrice`|`uint96`|The current stock price|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total exercise value|


### getOwnerTotalExerciseCostInternal

Get total exercise cost for an owner


```solidity
function getOwnerTotalExerciseCostInternal(address owner) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total exercise cost|


### isInTheMoneyInternal

Check if warrant is in the money


```solidity
function isInTheMoneyInternal(bytes32 tokenId, uint96 currentPrice) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|
|`currentPrice`|`uint96`|The current stock price|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if current price > strike price|


## Structs
### WarrantToken

```solidity
struct WarrantToken {
    bool isValid;
    address owner;
    uint96 quantity;
    address paymentCurrency;
    uint96 strikePrice;
}
```

### Layout

```solidity
struct Layout {
    mapping(bytes32 => WarrantToken) warrantTokens;
    mapping(address => bytes32[]) ownerWarrants;
    bool warrantInitialized;
}
```

