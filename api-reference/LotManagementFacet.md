# LotManagementFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/facets/LotManagementFacet.sol)

Facet for creating, adjusting, and managing lots in IssuedAsset

*Diamond facet providing lot creation and management functionality*


## Functions
### createLot

Create a new lot (initial issuance)


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
) external returns (bytes32 lotId, uint256 assignedCustomId);
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
) external returns (bytes32 newLotId);
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
function updateCustomId(bytes32 lotId, uint256 newCustomId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The hash-based lot ID to update|
|`newCustomId`|`uint256`|The new custom ID to assign|


### invalidateLot

Invalidate a lot (mark as invalid)


```solidity
function invalidateLot(bytes32 lotId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID to invalidate|


### _validateCompliance

Validate compliance through ComplianceFacet


```solidity
function _validateCompliance(address from, address to, bytes32 lotId, uint256 quantity) internal view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The sender address|
|`to`|`address`|The recipient address|
|`lotId`|`bytes32`|The lot ID being transferred|
|`quantity`|`uint256`|The quantity being transferred|


### _processIssuance

Process issuance through ComplianceFacet


```solidity
function _processIssuance(address to, bytes32 lotId, uint256 quantity, bytes memory data) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|The recipient address|
|`lotId`|`bytes32`|The lot ID being issued|
|`quantity`|`uint256`|The quantity being issued|
|`data`|`bytes`|Additional issuance data|


### _notifyLotAdjusted

Notify compliance about lot adjustment


```solidity
function _notifyLotAdjusted(bytes32 oldLotId, bytes32 newLotId) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oldLotId`|`bytes32`|The old lot ID|
|`newLotId`|`bytes32`|The new lot ID|


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
    IIssuedAsset.TransferType tType,
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
    IIssuedAsset.TransferType tType,
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

