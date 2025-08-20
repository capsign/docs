# CMXTransferFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/CMX/facets/CMXTransferFacet.sol)

Lot-based transfer functionality for CMX token

*Adapted from IssuedAsset TransferFacet for CMX token use*


## Functions
### transferLot

Transfer a specific lot or portion thereof


```solidity
function transferLot(
    bytes32 lotId,
    address to,
    uint256 quantity,
    CMXStorage.TransferType tType,
    uint256 newCostBasis,
    string memory uri,
    bytes memory data
) external returns (bytes32 newLotId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The ID of the lot to transfer from|
|`to`|`address`|The new owner|
|`quantity`|`uint256`|The quantity to transfer|
|`tType`|`CMXStorage.TransferType`|The type of transfer (SALE, GIFT, etc.)|
|`newCostBasis`|`uint256`|The new cost basis (used for SALE, INHERITANCE, REWARD)|
|`uri`|`string`|The URI of the lot|
|`data`|`bytes`|Additional data associated with the transfer|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`newLotId`|`bytes32`|The ID of the new lot created for the recipient|


### transferFromLots

Transfer CMX using FIFO lot strategy (for ERC-20 compatibility)


```solidity
function transferFromLots(address from, address to, uint256 amount, CMXStorage.TransferType tType)
    external
    returns (bool success);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The sender address|
|`to`|`address`|The recipient address|
|`amount`|`uint256`|The amount to transfer|
|`tType`|`CMXStorage.TransferType`|The transfer type (defaults to SALE for ERC-20 transfers)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`success`|`bool`|True if transfer succeeded|


### approveLot

Approve an operator for a specific lot


```solidity
function approveLot(bytes32 lotId, address operator, bool approved) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot identifier|
|`operator`|`address`|Address to approve|
|`approved`|`bool`|True to approve, false to revoke|


### setApprovalForAll

Approve an operator for all lots owned by caller


```solidity
function setApprovalForAll(address operator, bool approved) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`operator`|`address`|Address to approve|
|`approved`|`bool`|True to approve, false to revoke|


### isApprovedForLot

Check if operator is approved for a specific lot


```solidity
function isApprovedForLot(bytes32 lotId, address operator) external view returns (bool approved);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot identifier|
|`operator`|`address`|Address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`approved`|`bool`|True if approved|


### isApprovedForAll

Check if operator is approved for all lots of an owner


```solidity
function isApprovedForAll(address owner, address operator) external view returns (bool approved);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|
|`operator`|`address`|Address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`approved`|`bool`|True if approved for all|


### _createMintLot

*Create a new lot for minting*


```solidity
function _createMintLot(address to, uint256 amount) internal returns (bool success);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|The recipient address|
|`amount`|`uint256`|The amount to mint|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`success`|`bool`|True if mint succeeded|


### _transferUsingStrategy

*Transfer using the configured strategy*


```solidity
function _transferUsingStrategy(address from, address to, uint256 amount, CMXStorage.TransferType tType)
    internal
    returns (bool success);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The sender address|
|`to`|`address`|The recipient address|
|`amount`|`uint256`|The amount to transfer|
|`tType`|`CMXStorage.TransferType`|The transfer type|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`success`|`bool`|True if transfer succeeded|


### _getLotsInStrategyOrder

*Get lots in the order specified by the transfer strategy*


```solidity
function _getLotsInStrategyOrder(address owner, CMXStorage.TransferStrategy strategy)
    internal
    view
    returns (bytes32[] memory orderedLots);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner of the lots|
|`strategy`|`CMXStorage.TransferStrategy`|The transfer strategy to use|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`orderedLots`|`bytes32[]`|Array of lot IDs in strategy order|


### _sortLotsByFIFO

*Sort lots by FIFO (oldest acquisition date first)*


```solidity
function _sortLotsByFIFO(bytes32[] memory lots) internal view returns (bytes32[] memory);
```

### _sortLotsByLIFO

*Sort lots by LIFO (newest acquisition date first)*


```solidity
function _sortLotsByLIFO(bytes32[] memory lots) internal view returns (bytes32[] memory);
```

### _sortLotsByHighestCostBasis

*Sort lots by highest cost basis first*


```solidity
function _sortLotsByHighestCostBasis(bytes32[] memory lots) internal view returns (bytes32[] memory);
```

### _sortLotsByLowestCostBasis

*Sort lots by lowest cost basis first*


```solidity
function _sortLotsByLowestCostBasis(bytes32[] memory lots) internal view returns (bytes32[] memory);
```

### _sortLotsByAcquisitionDate

*Sort lots by acquisition date*


```solidity
function _sortLotsByAcquisitionDate(bytes32[] memory lots, bool ascending) internal view returns (bytes32[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lots`|`bytes32[]`|Array of lot IDs to sort|
|`ascending`|`bool`|True for oldest first, false for newest first|


### _sortLotsByCostBasis

*Sort lots by cost basis*


```solidity
function _sortLotsByCostBasis(bytes32[] memory lots, bool ascending) internal view returns (bytes32[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lots`|`bytes32[]`|Array of lot IDs to sort|
|`ascending`|`bool`|True for lowest first, false for highest first|


### _transferEntireLot

*Transfer an entire lot to new owner*


```solidity
function _transferEntireLot(bytes32 lotId, address from, address to, uint256 quantity, CMXStorage.TransferType tType)
    internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID to transfer|
|`from`|`address`|The current owner|
|`to`|`address`|The new owner|
|`quantity`|`uint256`|The quantity (should equal lot quantity)|
|`tType`|`CMXStorage.TransferType`|The transfer type|


### _transferPartialLot

*Transfer partial lot by creating new lot for recipient*


```solidity
function _transferPartialLot(bytes32 lotId, address from, address to, uint256 amount, CMXStorage.TransferType tType)
    internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID to transfer from|
|`from`|`address`|The current owner|
|`to`|`address`|The new owner|
|`amount`|`uint256`|The amount to transfer|
|`tType`|`CMXStorage.TransferType`|The transfer type|


### _isApprovedOrOwner

*Check if an address is approved or owner for a lot*


```solidity
function _isApprovedOrOwner(bytes32 lotId, address operator) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID to check|
|`operator`|`address`|The address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if approved or owner|


### _generateLotId

*Generate a lot ID*


```solidity
function _generateLotId(address owner, uint256 quantity, uint256 currentSupply, uint256 timestamp)
    internal
    view
    returns (bytes32);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|
|`quantity`|`uint256`|The quantity|
|`currentSupply`|`uint256`|The current supply|
|`timestamp`|`uint256`|The timestamp|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|The generated lot ID|


## Events
### LotTransferred

```solidity
event LotTransferred(
    bytes32 indexed lotId,
    address indexed from,
    address indexed to,
    uint256 quantity,
    string uri,
    bytes data,
    CMXStorage.TransferType tType,
    uint256 newCostBasis
);
```

### LotApproval

```solidity
event LotApproval(bytes32 indexed lotId, address indexed owner, address indexed spender, uint256 amount);
```

### ApprovalForAll

```solidity
event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
```

## Errors
### NotCompliant

```solidity
error NotCompliant();
```

### InvalidAddress

```solidity
error InvalidAddress();
```

### InsufficientQuantity

```solidity
error InsufficientQuantity();
```

### InvalidLot

```solidity
error InvalidLot();
```

### NotAuthorized

```solidity
error NotAuthorized();
```

### InsufficientBalance

```solidity
error InsufficientBalance();
```

