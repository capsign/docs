# VestingFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/facets/VestingFacet.sol)

**Inherits:**
[IComplianceCondition](/src/Tokenization/assets/interfaces/IComplianceCondition.sol/interface.IComplianceCondition.md)

Facet for managing token vesting schedules

*Implements vesting logic as a compliance condition facet*


## State Variables
### VESTING_STORAGE_POSITION

```solidity
bytes32 constant VESTING_STORAGE_POSITION = keccak256("capsign.storage.Vesting");
```


## Functions
### vestingStorage


```solidity
function vestingStorage() internal pure returns (VestingStorage storage vs);
```

### createVestingSchedule

Create a vesting schedule for a lot


```solidity
function createVestingSchedule(
    uint256 customLotId,
    address tokenHolder,
    uint256 totalAmount,
    uint256 startTime,
    uint256 duration,
    uint256 cliffDuration,
    bool revocable
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`customLotId`|`uint256`|The custom lot ID|
|`tokenHolder`|`address`|The address that will receive vested tokens|
|`totalAmount`|`uint256`|Total amount to vest|
|`startTime`|`uint256`|When vesting begins (timestamp)|
|`duration`|`uint256`|Total vesting duration in seconds|
|`cliffDuration`|`uint256`|Cliff period in seconds|
|`revocable`|`bool`|Whether the schedule can be revoked|


### releaseVestedTokens

Release vested tokens for a lot


```solidity
function releaseVestedTokens(uint256 customLotId) external returns (uint256 amount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`customLotId`|`uint256`|The custom lot ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The amount of tokens released|


### revokeVestingSchedule

Revoke a vesting schedule


```solidity
function revokeVestingSchedule(uint256 customLotId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`customLotId`|`uint256`|The custom lot ID|


### getReleasableAmount

Get the amount of tokens that can be released


```solidity
function getReleasableAmount(uint256 customLotId) public view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`customLotId`|`uint256`|The custom lot ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The amount of tokens that can be released|


### getVestedAmount

Get the total amount vested so far


```solidity
function getVestedAmount(uint256 customLotId) public view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`customLotId`|`uint256`|The custom lot ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total amount vested|


### getVestingSchedule

Get vesting schedule details


```solidity
function getVestingSchedule(uint256 customLotId) external view returns (VestingSchedule memory schedule);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`customLotId`|`uint256`|The custom lot ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`schedule`|`VestingSchedule`|The vesting schedule|


### getHolderVestingSchedules

Get all vesting schedules for a token holder


```solidity
function getHolderVestingSchedules(address tokenHolder) external view returns (uint256[] memory customLotIds);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenHolder`|`address`|The token holder address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`customLotIds`|`uint256[]`|Array of custom lot IDs with vesting schedules|


### validateVestingTransfer

Validate if a transfer complies with vesting rules


```solidity
function validateVestingTransfer(address from, address to, uint256 customLotId, uint256 amount)
    external
    view
    returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The sender address|
|`to`|`address`|The recipient address|
|`customLotId`|`uint256`|The custom lot ID|
|`amount`|`uint256`|The amount being transferred|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if transfer is allowed|


### processVestingTransfer

Process a transfer through vesting rules


```solidity
function processVestingTransfer(address from, address to, uint256 customLotId, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The sender address|
|`to`|`address`|The recipient address|
|`customLotId`|`uint256`|The custom lot ID|
|`amount`|`uint256`|The amount being transferred|


### migrateVestingLotState

Handle lot state migration during adjustments


```solidity
function migrateVestingLotState(bytes32 oldLotId, bytes32 newLotId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oldLotId`|`bytes32`|The old lot ID (hash-based)|
|`newLotId`|`bytes32`|The new lot ID (hash-based)|


### validateTransfer

Validate if a transfer complies with vesting rules (IComplianceCondition interface)


```solidity
function validateTransfer(address from, address to, uint256 lotId, uint256 amount) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The sender address|
|`to`|`address`|The recipient address|
|`lotId`|`uint256`|The lot ID|
|`amount`|`uint256`|The amount being transferred|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if transfer is allowed|


### processTransfer

Process a transfer through vesting rules (IComplianceCondition interface)


```solidity
function processTransfer(address from, address to, uint256 lotId, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The sender address|
|`to`|`address`|The recipient address|
|`lotId`|`uint256`|The lot ID|
|`amount`|`uint256`|The amount being transferred|


### processIssuance

Process issuance (IComplianceCondition interface)


```solidity
function processIssuance(address to, uint256 lotId, uint256 amount, bytes calldata data) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|The recipient address|
|`lotId`|`uint256`|The lot ID|
|`amount`|`uint256`|The amount being issued|
|`data`|`bytes`|Additional data (unused for vesting)|


### processRedemption

Process redemption (IComplianceCondition interface)


```solidity
function processRedemption(address from, uint256 lotId, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The sender address|
|`lotId`|`uint256`|The lot ID|
|`amount`|`uint256`|The amount being redeemed|


### migrateLotState

Migrate lot state (IComplianceCondition interface)


```solidity
function migrateLotState(bytes32 oldLotId, bytes32 newLotId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oldLotId`|`bytes32`|The old lot ID|
|`newLotId`|`bytes32`|The new lot ID|


### conditionName

Get the condition name (IComplianceCondition interface)


```solidity
function conditionName() external pure returns (string memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|The name of this condition|


## Events
### VestingScheduleCreated

```solidity
event VestingScheduleCreated(
    address indexed tokenHolder,
    uint256 indexed customLotId,
    uint256 totalAmount,
    uint256 startTime,
    uint256 duration,
    uint256 cliffDuration,
    bool revocable
);
```

### TokensReleased

```solidity
event TokensReleased(address indexed tokenHolder, uint256 indexed customLotId, uint256 amount);
```

### VestingScheduleRevoked

```solidity
event VestingScheduleRevoked(address indexed tokenHolder, uint256 indexed customLotId);
```

## Errors
### Vesting_NoVestingSchedule

```solidity
error Vesting_NoVestingSchedule();
```

### Vesting_AlreadyRevoked

```solidity
error Vesting_AlreadyRevoked();
```

### Vesting_NotRevocable

```solidity
error Vesting_NotRevocable();
```

### Vesting_TransferViolatesVesting

```solidity
error Vesting_TransferViolatesVesting();
```

### Vesting_InvalidDuration

```solidity
error Vesting_InvalidDuration();
```

### Vesting_InvalidCliffDuration

```solidity
error Vesting_InvalidCliffDuration();
```

## Structs
### VestingSchedule

```solidity
struct VestingSchedule {
    address tokenHolder;
    uint256 totalAmount;
    uint256 releasedAmount;
    uint256 startTime;
    uint256 duration;
    uint256 cliffDuration;
    bool revocable;
    bool revoked;
}
```

### VestingStorage

```solidity
struct VestingStorage {
    mapping(uint256 => VestingSchedule) schedules;
    mapping(address => uint256[]) holderSchedules;
}
```

