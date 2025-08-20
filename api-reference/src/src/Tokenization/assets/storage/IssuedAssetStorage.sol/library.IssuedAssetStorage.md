# IssuedAssetStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/storage/IssuedAssetStorage.sol)

Diamond storage library for IssuedAsset functionality

*Uses deterministic storage slots to avoid storage collisions*


## State Variables
### ISSUED_ASSET_STORAGE_POSITION

```solidity
bytes32 constant ISSUED_ASSET_STORAGE_POSITION = keccak256("capsign.storage.issued_asset");
```


## Functions
### layout

Get the storage layout


```solidity
function layout() internal pure returns (Layout storage l);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`l`|`Layout`|The storage layout struct|


### initializeStorage

Initialize the storage with default values


```solidity
function initializeStorage(address _owner, string memory _name, string memory _symbol, string memory _baseURI)
    internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_owner`|`address`|The initial owner of the asset|
|`_name`|`string`|The name of the asset|
|`_symbol`|`string`|The symbol of the asset|
|`_baseURI`|`string`|The base URI for metadata|


### generateHashLotId

Generate a hash-based lot ID


```solidity
function generateHashLotId(address owner, uint256 quantity, uint256 currentSupply, uint256 timestamp)
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


### generateLotId

Generate a new lot ID (alternative method)


```solidity
function generateLotId() internal view returns (bytes32);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|A unique lot ID|


### isApprovedOrOwner

Check if caller is authorized for lot operations


```solidity
function isApprovedOrOwner(bytes32 lotId, address operator) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID to check authorization for|
|`operator`|`address`|The address to check authorization for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if authorized|


### addComplianceCondition


```solidity
function addComplianceCondition(address condition, string memory conditionName) internal;
```

### removeComplianceCondition


```solidity
function removeComplianceCondition(address condition, string memory conditionName) internal;
```

### addLotCondition


```solidity
function addLotCondition(uint256 customLotId, address condition) internal;
```

### getApplicableConditions


```solidity
function getApplicableConditions(uint256 customLotId) internal view returns (address[] memory);
```

### requireOwner


```solidity
function requireOwner() internal view;
```

### requireAdmin


```solidity
function requireAdmin() internal view;
```

### requireNotPaused


```solidity
function requireNotPaused() internal view;
```

### requireValidLot


```solidity
function requireValidLot(bytes32 lotId) internal view;
```

### requireNotFrozen


```solidity
function requireNotFrozen(address account, bytes32 lotId) internal view;
```

### _authority


```solidity
function _authority() private view returns (address);
```

### _hasTransferAgentRole


```solidity
function _hasTransferAgentRole(address account) private view returns (bool);
```

### _hasOfferingOperatorRole


```solidity
function _hasOfferingOperatorRole(address account) private view returns (bool);
```

## Structs
### Layout

```solidity
struct Layout {
    string name;
    string symbol;
    uint8 decimals;
    uint256 totalSupply;
    string baseURI;
    mapping(bytes32 => IIssuedAsset.Lot) lots;
    mapping(address => bytes32[]) ownerLots;
    mapping(address => uint256) balances;
    bool paused;
    mapping(address => bool) agents;
    address owner;
    uint256 nextCustomId;
    mapping(uint256 => bytes32) customIdToLotId;
    mapping(bytes32 => uint256) lotIdToCustomId;
    address[] complianceConditions;
    mapping(address => bool) conditionRegistered;
    mapping(uint256 => address[]) lotConditions;
    mapping(uint256 => bool) lotHasSpecificConditions;
    mapping(string => address) conditionNameToAddress;
    mapping(address => bool) frozenAccounts;
    mapping(bytes32 => bool) frozenLots;
    mapping(bytes32 => mapping(address => uint256)) lotApprovals;
    mapping(address => mapping(address => bool)) operatorApprovals;
    bool initialized;
}
```

