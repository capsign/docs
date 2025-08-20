# ICompliance
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/interfaces/ICompliance.sol)

Interface for embedded compliance functionality in asset diamonds

*This interface defines methods for managing compliance conditions and validating
transfers, issuances, and redemptions within the diamond architecture*


## Functions
### addComplianceCondition

Add a compliance condition to the asset


```solidity
function addComplianceCondition(address condition, string memory conditionName) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`condition`|`address`|The address of the condition contract|
|`conditionName`|`string`|The name of the condition for lookup|


### removeComplianceCondition

Remove a compliance condition from the asset


```solidity
function removeComplianceCondition(address condition, string memory conditionName) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`condition`|`address`|The address of the condition contract|
|`conditionName`|`string`|The name of the condition|


### addLotCondition

Add a condition to a specific lot


```solidity
function addLotCondition(uint256 customLotId, address condition) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`customLotId`|`uint256`|The custom lot ID|
|`condition`|`address`|The address of the condition|


### removeLotCondition

Remove a condition from a specific lot


```solidity
function removeLotCondition(uint256 customLotId, address condition) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`customLotId`|`uint256`|The custom lot ID|
|`condition`|`address`|The address of the condition|


### getAllComplianceConditions

Get all global compliance conditions


```solidity
function getAllComplianceConditions() external view returns (address[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Array of condition addresses|


### getLotConditions

Get conditions for a specific lot


```solidity
function getLotConditions(uint256 customLotId) external view returns (address[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`customLotId`|`uint256`|The custom lot ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Array of condition addresses|


### getConditionByName

Get condition address by name


```solidity
function getConditionByName(string memory conditionName) external view returns (address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`conditionName`|`string`|The name of the condition|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The condition address|


### setOrUpdateCustomLotId

Set or update custom lot ID for a hash-based lot


```solidity
function setOrUpdateCustomLotId(bytes32 hashLotId, uint256 desiredId) external returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`hashLotId`|`bytes32`|The hash-based lot ID|
|`desiredId`|`uint256`|The desired custom ID (0 for auto-increment)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The actual custom ID assigned|


### getCustomLotId

Get custom lot ID for a hash-based lot


```solidity
function getCustomLotId(bytes32 hashLotId) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`hashLotId`|`bytes32`|The hash-based lot ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The custom lot ID (0 if not set)|


### getHashLotId

Get hash lot ID from custom ID


```solidity
function getHashLotId(uint256 customId) external view returns (bytes32);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`customId`|`uint256`|The custom lot ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|The hash-based lot ID|


### validateTransfer

Validate if a transfer is compliant


```solidity
function validateTransfer(address from, address to, bytes32 hashLotId, uint256 quantity) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The sender address|
|`to`|`address`|The recipient address|
|`hashLotId`|`bytes32`|The hash-based lot ID|
|`quantity`|`uint256`|The quantity being transferred|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if transfer is compliant|


### processTransfer

Process a transfer through compliance conditions


```solidity
function processTransfer(address from, address to, bytes32 hashLotId, uint256 quantity) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The sender address|
|`to`|`address`|The recipient address|
|`hashLotId`|`bytes32`|The hash-based lot ID|
|`quantity`|`uint256`|The quantity being transferred|


### processIssuance

Process an issuance through compliance conditions


```solidity
function processIssuance(address to, bytes32 hashLotId, uint256 quantity, bytes memory data) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|The recipient address|
|`hashLotId`|`bytes32`|The hash-based lot ID|
|`quantity`|`uint256`|The quantity being issued|
|`data`|`bytes`|Additional data for the issuance|


### processRedemption

Process a redemption through compliance conditions


```solidity
function processRedemption(address from, bytes32 hashLotId, uint256 quantity) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The sender address|
|`hashLotId`|`bytes32`|The hash-based lot ID|
|`quantity`|`uint256`|The quantity being redeemed|


### notifyLotAdjusted

Notify conditions about lot adjustment


```solidity
function notifyLotAdjusted(bytes32 oldLotId, bytes32 newLotId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oldLotId`|`bytes32`|The old hash-based lot ID|
|`newLotId`|`bytes32`|The new hash-based lot ID|


### freezeAccount

Freeze an account, preventing transfers from this account


```solidity
function freezeAccount(address account) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address to freeze|


### unfreezeAccount

Unfreeze a previously frozen account


```solidity
function unfreezeAccount(address account) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address to unfreeze|


### freezeLot

Freeze a specific lot, preventing transfers


```solidity
function freezeLot(bytes32 lotId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The ID of the lot to freeze|


### unfreezeLot

Unfreeze a previously frozen lot


```solidity
function unfreezeLot(bytes32 lotId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The ID of the lot to unfreeze|


### isAccountFrozen

Check if an account is frozen


```solidity
function isAccountFrozen(address account) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the account is frozen|


### isLotFrozen

Check if a lot is frozen


```solidity
function isLotFrozen(bytes32 lotId) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the lot is frozen|


### getComplianceStatus

Get compliance status for an account


```solidity
function getComplianceStatus(address account)
    external
    view
    returns (bool frozenAccount, uint256 lotCount, uint256 totalBalance);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The account to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`frozenAccount`|`bool`|Whether the account is frozen|
|`lotCount`|`uint256`|Number of lots owned|
|`totalBalance`|`uint256`|Total balance across all lots|


### batchFreezeAccounts

Batch freeze multiple accounts


```solidity
function batchFreezeAccounts(address[] calldata accounts) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`accounts`|`address[]`|Array of addresses to freeze|


### batchUnfreezeAccounts

Batch unfreeze multiple accounts


```solidity
function batchUnfreezeAccounts(address[] calldata accounts) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`accounts`|`address[]`|Array of addresses to unfreeze|


### batchFreezeLots

Batch freeze multiple lots


```solidity
function batchFreezeLots(bytes32[] calldata lotIds) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotIds`|`bytes32[]`|Array of lot IDs to freeze|


### batchUnfreezeLots

Batch unfreeze multiple lots


```solidity
function batchUnfreezeLots(bytes32[] calldata lotIds) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotIds`|`bytes32[]`|Array of lot IDs to unfreeze|


## Events
### ComplianceConditionAdded

```solidity
event ComplianceConditionAdded(address indexed condition, string name);
```

### ComplianceConditionRemoved

```solidity
event ComplianceConditionRemoved(address indexed condition, string name);
```

### LotConditionAdded

```solidity
event LotConditionAdded(uint256 indexed customLotId, address indexed condition);
```

### LotConditionRemoved

```solidity
event LotConditionRemoved(uint256 indexed customLotId, address indexed condition);
```

### CustomLotIdSet

```solidity
event CustomLotIdSet(bytes32 indexed hashLotId, uint256 customId);
```

### CustomLotIdUpdated

```solidity
event CustomLotIdUpdated(bytes32 indexed hashLotId, uint256 oldCustomId, uint256 newCustomId);
```

### AccountFrozen

```solidity
event AccountFrozen(address indexed account, bool frozen);
```

### LotFrozen

```solidity
event LotFrozen(bytes32 indexed lotId, bool frozen);
```

