# EsoStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/storage/EsoStorage.sol)

Diamond storage library for Employee Stock Option functionality

*Uses deterministic storage slots to avoid storage collisions with IssuedAsset*


## State Variables
### ESO_STORAGE_POSITION

```solidity
bytes32 constant ESO_STORAGE_POSITION = keccak256("capsign.storage.eso");
```


## Functions
### layout

Get the ESO storage layout


```solidity
function layout() internal pure returns (Layout storage l);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`l`|`Layout`|The storage layout struct|


### initializeEsoStorage

Initialize the ESO storage with default values


```solidity
function initializeEsoStorage() internal;
```

### createOptionTokenInternal

Create a new option token


```solidity
function createOptionTokenInternal(
    bytes32 tokenId,
    address owner,
    uint96 quantity,
    address paymentCurrency,
    uint96 strikePrice,
    uint256 vestingStartTime,
    uint256 cliffTime,
    uint256 vestingEndTime
) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The unique token ID|
|`owner`|`address`|The owner address|
|`quantity`|`uint96`|The number of options|
|`paymentCurrency`|`address`|The payment currency|
|`strikePrice`|`uint96`|The strike price|
|`vestingStartTime`|`uint256`|The vesting start time|
|`cliffTime`|`uint256`|The cliff time|
|`vestingEndTime`|`uint256`|The vesting end time|


### invalidateOptionTokenInternal

Invalidate an option token


```solidity
function invalidateOptionTokenInternal(bytes32 tokenId) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID to invalidate|


### exerciseOptionInternal

Exercise options (reduce quantity)


```solidity
function exerciseOptionInternal(bytes32 tokenId, uint96 exerciseQuantity) internal returns (uint96 remainingQuantity);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|
|`exerciseQuantity`|`uint96`|The quantity to exercise|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`remainingQuantity`|`uint96`|The remaining quantity after exercise|


### getOptionTokenInternal

Get option token information


```solidity
function getOptionTokenInternal(bytes32 tokenId) internal view returns (OptionToken memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`OptionToken`|The option token struct|


### isOptionTokenValidInternal

Check if an option token is valid


```solidity
function isOptionTokenValidInternal(bytes32 tokenId) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if valid|


### getOwnerOptionsInternal

Get all option tokens for an owner


```solidity
function getOwnerOptionsInternal(address owner) internal view returns (bytes32[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32[]`|Array of token IDs|


### getOwnerOptionCountInternal

Get the number of option tokens for an owner


```solidity
function getOwnerOptionCountInternal(address owner) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The count of option tokens|


### calculateVestedQuantityInternal

Calculate vested quantity for an option token


```solidity
function calculateVestedQuantityInternal(bytes32 tokenId) internal view returns (uint96);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint96`|The vested quantity as of current time|


### calculateVestedQuantityAtTimeInternal

Calculate vested quantity at a specific time


```solidity
function calculateVestedQuantityAtTimeInternal(bytes32 tokenId, uint256 timestamp) internal view returns (uint96);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|
|`timestamp`|`uint256`|The timestamp to calculate vesting for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint96`|The vested quantity at the specified time|


### generateOptionTokenIdInternal

Generate an option token ID


```solidity
function generateOptionTokenIdInternal(
    address owner,
    uint96 quantity,
    address paymentCurrency,
    uint96 strikePrice,
    uint256 vestingStartTime,
    uint256 cliffTime,
    uint256 vestingEndTime
) internal view returns (bytes32);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|
|`quantity`|`uint96`|The quantity|
|`paymentCurrency`|`address`|The payment currency|
|`strikePrice`|`uint96`|The strike price|
|`vestingStartTime`|`uint256`|The vesting start time|
|`cliffTime`|`uint256`|The cliff time|
|`vestingEndTime`|`uint256`|The vesting end time|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|The generated token ID|


### isEsoInitialized

Check if ESO is initialized


```solidity
function isEsoInitialized() internal view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if initialized|


### updateOptionTokenOwnerInternal

Update option token owner (for transfers)


```solidity
function updateOptionTokenOwnerInternal(bytes32 tokenId, address newOwner) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|
|`newOwner`|`address`|The new owner address|


### getOwnerTotalQuantityInternal

Get total quantity of options for an owner


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


### getOwnerTotalVestedQuantityInternal

Get total vested quantity for an owner


```solidity
function getOwnerTotalVestedQuantityInternal(address owner) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total vested quantity|


### getOwnerTotalExerciseValueInternal

Get total exercise value for an owner


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


### calculateExerciseValueInternal

Calculate exercise value for an option token


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
|`<none>`|`uint256`|The exercise value|


### isInTheMoneyInternal

Check if option is in the money


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
### OptionToken

```solidity
struct OptionToken {
    bool isValid;
    address owner;
    uint96 quantity;
    address paymentCurrency;
    uint96 strikePrice;
    uint96 totalGrantAmount;
    uint256 vestingStartTime;
    uint256 cliffTime;
    uint256 vestingEndTime;
}
```

### Layout

```solidity
struct Layout {
    mapping(bytes32 => OptionToken) optionTokens;
    mapping(address => bytes32[]) ownerOptions;
    bool esoInitialized;
    address compensationFactory;
    address rule701Diamond;
}
```

