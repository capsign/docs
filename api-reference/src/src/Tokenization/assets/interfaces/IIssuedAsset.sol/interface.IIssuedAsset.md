# IIssuedAsset
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/interfaces/IIssuedAsset.sol)

**Author:**
CapSign Inc.

Interface for issued assets.


## Functions
### name

*Returns the name of the asset.*


```solidity
function name() external view returns (string memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|The name of the asset.|


### symbol

*Returns the symbol of the asset.*


```solidity
function symbol() external view returns (string memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|The symbol of the asset.|


### transfer

*Transfers a lot or partial lot from sender to 'to', with specified transfer type.*


```solidity
function transfer(
    bytes32 lotId,
    address to,
    uint256 quantity,
    TransferType tType,
    uint256 newCostBasis,
    string memory uri,
    bytes memory data
) external returns (bytes32 newLotId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The ID of the lot to transfer from.|
|`to`|`address`|The new owner.|
|`quantity`|`uint256`|The quantity to transfer.|
|`tType`|`TransferType`|The type of transfer (SALE, GIFT, etc.)|
|`newCostBasis`|`uint256`|The new cost basis (used for SALE, INHERITANCE, REWARD)|
|`uri`|`string`|The URI of the lot.|
|`data`|`bytes`|Additional data associated with the transfer.|


### transferFrom

*Transfers a lot or partial lot from 'from' to 'to', with specified transfer type.*


```solidity
function transferFrom(
    bytes32 lotId,
    address from,
    address to,
    uint256 quantity,
    TransferType tType,
    uint256 newCostBasis,
    string memory uri,
    bytes memory data
) external returns (bytes32 newLotId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The ID of the lot to transfer from.|
|`from`|`address`|The current owner of the lot.|
|`to`|`address`|The new owner.|
|`quantity`|`uint256`|The quantity to transfer.|
|`tType`|`TransferType`|The type of transfer (SALE, GIFT, etc.)|
|`newCostBasis`|`uint256`|The new cost basis (used for SALE, INHERITANCE, REWARD)|
|`uri`|`string`|The URI of the lot.|
|`data`|`bytes`|Additional data associated with the transfer.|


### getLot

*Returns the stored data for a specific lot.*


```solidity
function getLot(bytes32 lotId)
    external
    view
    returns (
        bytes32 parentLotId,
        bool isValid,
        uint256 quantity,
        address paymentCurrency,
        uint256 costBasis,
        uint256 acquisitionDate,
        uint256 lastUpdate,
        TransferType tType,
        string memory uri,
        bytes memory data
    );
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID to query.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`parentLotId`|`bytes32`|Hash of the parent lot, or 0x0 if none.|
|`isValid`|`bool`|Whether the lot is active.|
|`quantity`|`uint256`|The quantity.|
|`paymentCurrency`|`address`|The payment currency for this lot.|
|`costBasis`|`uint256`|The cost basis.|
|`acquisitionDate`|`uint256`|The original acquisition timestamp (unmodified).|
|`lastUpdate`|`uint256`|The last time this lot was updated.|
|`tType`|`TransferType`|The transfer type.|
|`uri`|`string`|The URI of the lot.|
|`data`|`bytes`|Additional data associated with the lot.|


### addAgent

Adds an agent to the issued asset.


```solidity
function addAgent(address _agent) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_agent`|`address`|The address of the agent to add.|


### removeAgent

Removes an agent from the issued asset.


```solidity
function removeAgent(address _agent) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_agent`|`address`|The address of the agent to remove.|


### isAgent

Checks if an address is an agent.


```solidity
function isAgent(address _agent) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_agent`|`address`|The address to check.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the address is an agent, false otherwise.|


### createLot

*Creates a brand-new lot (e.g., an initial issuance) for a user.*


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
|`owner`|`address`|The owner of the lot.|
|`quantity`|`uint256`|The total quantity for this lot.|
|`paymentCurrency`|`address`|The payment currency for this lot.|
|`costBasis`|`uint256`|The cost basis per unit.|
|`acquisitionDate`|`uint256`|The original acquisition date.|
|`uri`|`string`|The URI of the lot.|
|`data`|`bytes`|Additional data associated with the lot.|
|`customId`|`uint256`|Optional custom ID (0 for auto-assignment).|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The unique ID of the new lot.|
|`assignedCustomId`|`uint256`|The assigned custom ID.|


### adjustLot

*Creates a new lot as a child of an old lot, typically used for spin-offs,
cost basis corrections, partial reclassifications, etc.*


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
|`oldLotId`|`bytes32`|The old lot to be adjusted.|
|`newQuantity`|`uint256`|The quantity for the new lot.|
|`newCostBasis`|`uint256`|The cost basis for the new lot.|
|`newPaymentCurrency`|`address`|The payment currency for the new lot (zero address to keep the original).|
|`newAcquisitionDate`|`uint256`|The acquisition date for the new lot (0 to keep the original).|
|`newUri`|`string`|The URI for the new lot.|
|`newData`|`bytes`|Additional data associated with the new lot.|
|`reason`|`string`|A short string explaining the adjustment type.|
|`customId`|`uint256`|Optional custom ID (0 for auto-assignment).|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`newLotId`|`bytes32`|The ID of the newly created lot.|


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


### balanceOf

Returns the balance of a specific address


```solidity
function balanceOf(address account) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The balance of the address|


## Events
### BaseURIUpdated
Emitted when the base URI is updated


```solidity
event BaseURIUpdated(string newBaseURI);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newBaseURI`|`string`|The new base URI|

### LotCreated
Emitted when a new lot is created (initial issuance or partial sale).


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
    TransferType tType,
    uint256 newCostBasis
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The user who owns this lot.|
|`lotId`|`bytes32`|The unique hash-based identifier of the lot.|
|`parentLotId`|`bytes32`|The lotId from which this lot originated (0x0 if none).|
|`quantity`|`uint256`|The quantity in this new lot.|
|`paymentCurrency`|`address`|The payment currency used for this lot.|
|`costBasis`|`uint256`|The cost basis per unit.|
|`acquisitionDate`|`uint256`|The original acquisition date of the lot.|
|`lastUpdate`|`uint256`|The timestamp when this lot was created/updated.|
|`uri`|`string`|The URI of the lot.|
|`data`|`bytes`|Additional data associated with the lot.|
|`tType`|`TransferType`|The type of transfer.|
|`newCostBasis`|`uint256`|The new cost basis (if applicable).|

### LotTransferred
Emitted when a lot is transferred from one owner to another.


```solidity
event LotTransferred(
    bytes32 indexed lotId,
    address indexed from,
    address indexed to,
    uint256 quantity,
    string uri,
    bytes data,
    TransferType tType,
    uint256 newCostBasis
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The ID of the lot being transferred.|
|`from`|`address`|The address transferring the lot.|
|`to`|`address`|The address receiving the lot.|
|`quantity`|`uint256`|The quantity being transferred.|
|`uri`|`string`|The URI of the lot.|
|`data`|`bytes`|Additional data associated with the transfer.|
|`tType`|`TransferType`|The type of transfer.|
|`newCostBasis`|`uint256`|The new cost basis (if applicable).|

### LotInvalidated
Emitted when a lot is invalidated.


```solidity
event LotInvalidated(bytes32 indexed lotId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The ID of the lot being invalidated.|

### ApprovalForAll
Emitted when an operator is approved for all owned lots.


```solidity
event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner of the lot.|
|`operator`|`address`|The operator being approved.|
|`approved`|`bool`|Whether the operator is approved.|

### LotApproval
Emitted when a lot is approved for a specific operation.


```solidity
event LotApproval(bytes32 indexed lotId, address indexed owner, address indexed spender, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The ID of the lot being approved.|
|`owner`|`address`|The owner of the lot.|
|`spender`|`address`|The spender being approved.|
|`amount`|`uint256`|The amount being approved.|

### LotsAreBeingMerged
Emitted when lots are being merged


```solidity
event LotsAreBeingMerged(
    bytes32[] sourceLots, bytes32 indexed newLotId, address indexed owner, uint256 totalQuantity, bytes data
);
```

### SplitCompletedOrMergeSrcLotsInvalid
Emitted when a merge or split operation is completed


```solidity
event SplitCompletedOrMergeSrcLotsInvalid(bytes32[] lotIds);
```

### CustomIdUpdated
Emitted when a custom ID is updated for a lot


```solidity
event CustomIdUpdated(bytes32 indexed lotId, uint256 oldCustomId, uint256 newCustomId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The hash-based lot ID|
|`oldCustomId`|`uint256`|The previous custom ID (0 if none)|
|`newCustomId`|`uint256`|The new custom ID assigned|

### AgentAdded
Emitted when an agent is added


```solidity
event AgentAdded(address indexed agent);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`agent`|`address`|Address of the agent that was added|

### AgentRemoved
Emitted when an agent is removed


```solidity
event AgentRemoved(address indexed agent);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`agent`|`address`|Address of the agent that was removed|

### PausedSet
Emitted when the paused state is set


```solidity
event PausedSet(bool indexed paused);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`paused`|`bool`|The new paused state|

### TransferControllerAdded
Emitted when transfer controller is updated


```solidity
event TransferControllerAdded(address indexed transferControllerAddress);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`transferControllerAddress`|`address`|The address of the new transfer controller|

### LotAdjusted
Emitted when a lot is adjusted, creating a new lot from an old one.


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
    TransferType tType,
    uint256 adjustedCostBasis
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oldLotId`|`bytes32`|The ID of the original lot.|
|`newLotId`|`bytes32`|The ID of the new adjusted lot.|
|`operator`|`address`|The address performing the adjustment.|
|`newQuantity`|`uint256`|The new quantity of the adjusted lot.|
|`newCostBasis`|`uint256`|The new cost basis of the adjusted lot.|
|`paymentCurrency`|`address`|The payment currency for the adjusted lot.|
|`newAcquisitionDate`|`uint256`|The new acquisition date for the adjusted lot.|
|`newUri`|`string`|The URI of the new lot.|
|`newData`|`bytes`|Additional data associated with the new lot.|
|`reason`|`string`|The reason for the adjustment.|
|`tType`|`TransferType`|The type of transfer.|
|`adjustedCostBasis`|`uint256`|Any additional cost basis adjustment.|

### AccountFrozen
Emitted when an account is frozen


```solidity
event AccountFrozen(address indexed account, bool frozen);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address of the account that was frozen|
|`frozen`|`bool`|The new frozen state|

### LotFrozen
Emitted when a lot is frozen


```solidity
event LotFrozen(bytes32 indexed lotId, bool frozen);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The ID of the lot that was frozen|
|`frozen`|`bool`|The new frozen state|

## Structs
### Lot

```solidity
struct Lot {
    bytes32 parentLotId;
    uint256 quantity;
    address paymentCurrency;
    uint256 costBasis;
    uint256 acquisitionDate;
    uint256 lastUpdate;
    bool isValid;
    address owner;
    TransferType tType;
    string uri;
    bytes data;
}
```

## Enums
### TransferType

```solidity
enum TransferType {
    INTERNAL,
    SALE,
    GIFT,
    INHERITANCE,
    REWARD
}
```

