# CMXLotManagementFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/CMX/facets/CMXLotManagementFacet.sol)

Lot creation and management for CMX token

*Adapted from IssuedAsset LotManagementFacet for CMX token use*


## Functions
### onlyOwner


```solidity
modifier onlyOwner();
```

### validLot


```solidity
modifier validLot(bytes32 lotId);
```

### createLot

Create a new CMX lot


```solidity
function createLot(
    address owner,
    uint256 quantity,
    address paymentCurrency,
    uint256 costBasis,
    uint256 acquisitionDate,
    string memory uri,
    bytes memory data,
    uint256 customId
) external onlyOwner returns (bytes32 lotId, uint256 assignedCustomId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner of the lot|
|`quantity`|`uint256`|The total quantity for this lot|
|`paymentCurrency`|`address`|The payment currency for this lot|
|`costBasis`|`uint256`|The cost basis per unit|
|`acquisitionDate`|`uint256`|The original acquisition date|
|`uri`|`string`|The URI of the lot|
|`data`|`bytes`|Additional data associated with the lot|
|`customId`|`uint256`|Optional custom ID (0 for auto-assignment)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The unique ID of the new lot|
|`assignedCustomId`|`uint256`|The assigned custom ID|


### adjustLot

Adjust an existing lot to create a new lot with different parameters


```solidity
function adjustLot(
    bytes32 oldLotId,
    uint256 newQuantity,
    uint256 newCostBasis,
    address newPaymentCurrency,
    uint256 newAcquisitionDate,
    string memory newUri,
    bytes memory newData,
    string calldata reason,
    uint256 customId
) external validLot(oldLotId) returns (bytes32 newLotId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oldLotId`|`bytes32`|The old lot to be adjusted|
|`newQuantity`|`uint256`|The quantity for the new lot|
|`newCostBasis`|`uint256`|The cost basis for the new lot|
|`newPaymentCurrency`|`address`|The payment currency for the new lot (zero address to keep the original)|
|`newAcquisitionDate`|`uint256`|The acquisition date for the new lot (0 to keep the original)|
|`newUri`|`string`|The URI for the new lot|
|`newData`|`bytes`|Additional data associated with the new lot|
|`reason`|`string`|A short string explaining the adjustment type|
|`customId`|`uint256`|Optional custom ID (0 for auto-assignment)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`newLotId`|`bytes32`|The ID of the newly created lot|


### updateCustomId

Update the custom ID for an existing lot


```solidity
function updateCustomId(bytes32 lotId, uint256 newCustomId) external validLot(lotId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The hash-based lot ID to update|
|`newCustomId`|`uint256`|The new custom ID to assign|


### invalidateLot

Invalidate a lot (mark as invalid)


```solidity
function invalidateLot(bytes32 lotId) external validLot(lotId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID to invalidate|


### _generateLotId

*Generate a hash-based lot ID*


```solidity
function _generateLotId(address owner, uint256 quantity, uint256 currentSupply, uint256 timestamp)
    internal
    view
    returns (bytes32 lotId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner of the lot|
|`quantity`|`uint256`|The quantity of the lot|
|`currentSupply`|`uint256`|The current total supply|
|`timestamp`|`uint256`|The current timestamp|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The generated lot ID|


### _lotExists

*Check if a lot exists and is valid*


```solidity
function _lotExists(bytes32 lotId) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the lot exists and is valid|


## Events
### LotCreated

```solidity
event LotCreated(
    address indexed owner,
    bytes32 indexed lotId,
    bytes32 indexed parentLotId,
    uint256 quantity,
    address paymentCurrency,
    uint256 costBasis,
    uint256 acquisitionDate,
    uint256 lastUpdate,
    string uri,
    bytes data,
    CMXStorage.TransferType tType,
    uint256 newCostBasis
);
```

### LotAdjusted

```solidity
event LotAdjusted(
    bytes32 indexed oldLotId,
    bytes32 indexed newLotId,
    address operator,
    uint256 newQuantity,
    uint256 newCostBasis,
    address paymentCurrency,
    uint256 newAcquisitionDate,
    string newUri,
    bytes newData,
    string reason,
    CMXStorage.TransferType tType,
    uint256 adjustedCostBasis
);
```

### CustomIdUpdated

```solidity
event CustomIdUpdated(bytes32 indexed lotId, uint256 oldCustomId, uint256 newCustomId);
```

### LotInvalidated

```solidity
event LotInvalidated(bytes32 indexed lotId);
```

## Errors
### InvalidOwner

```solidity
error InvalidOwner();
```

### InvalidQuantity

```solidity
error InvalidQuantity();
```

### CustomIdInUse

```solidity
error CustomIdInUse();
```

### NotAuthorized

```solidity
error NotAuthorized();
```

### InvalidLot

```solidity
error InvalidLot();
```

