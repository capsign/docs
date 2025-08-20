# LockupFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/facets/LockupFacet.sol)

**Inherits:**
[IComplianceCondition](/src/Tokenization/assets/interfaces/IComplianceCondition.sol/interface.IComplianceCondition.md)

Facet for managing token lockup periods

*Implements lockup logic as a compliance condition facet*


## State Variables
### LOCKUP_STORAGE_POSITION

```solidity
bytes32 constant LOCKUP_STORAGE_POSITION = keccak256("capsign.storage.Lockup");
```


## Functions
### lockupStorage


```solidity
function lockupStorage() internal pure returns (LockupStorage storage ls);
```

### createLotLockup

Create a lockup period for a specific lot


```solidity
function createLotLockup(uint256 customLotId, uint256 unlockTime, uint256 lockedAmount, string memory reason)
    external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`customLotId`|`uint256`|The custom lot ID|
|`unlockTime`|`uint256`|When the lockup expires (timestamp)|
|`lockedAmount`|`uint256`|Amount of tokens to lock|
|`reason`|`string`|Reason for the lockup|


### createAccountLockup

Create a lockup period for an account (affects all their tokens)


```solidity
function createAccountLockup(address account, uint256 unlockTime, uint256 lockedAmount, string memory reason)
    external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The account to lock tokens for|
|`unlockTime`|`uint256`|When the lockup expires (timestamp)|
|`lockedAmount`|`uint256`|Amount of tokens to lock|
|`reason`|`string`|Reason for the lockup|


### releaseLotLockup

Release an expired lot lockup


```solidity
function releaseLotLockup(uint256 customLotId, uint256 lockupIndex) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`customLotId`|`uint256`|The custom lot ID|
|`lockupIndex`|`uint256`|The index of the lockup to release|


### releaseAccountLockup

Release an expired account lockup


```solidity
function releaseAccountLockup(address account, uint256 lockupIndex) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The account address|
|`lockupIndex`|`uint256`|The index of the lockup to release|


### getTotalLockedByLot

Get total locked amount for a lot


```solidity
function getTotalLockedByLot(uint256 customLotId) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`customLotId`|`uint256`|The custom lot ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Total amount currently locked|


### getTotalLockedByAccount

Get total locked amount for an account


```solidity
function getTotalLockedByAccount(address account) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The account address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Total amount currently locked|


### getLotLockups

Get all lockup periods for a lot


```solidity
function getLotLockups(uint256 customLotId) external view returns (LockupPeriod[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`customLotId`|`uint256`|The custom lot ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`LockupPeriod[]`|Array of lockup periods|


### getAccountLockups

Get all lockup periods for an account


```solidity
function getAccountLockups(address account) external view returns (LockupPeriod[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The account address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`LockupPeriod[]`|Array of lockup periods|


### validateLockupTransfer

Validate if a transfer complies with lockup rules


```solidity
function validateLockupTransfer(address from, address to, uint256 customLotId, uint256 amount)
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


### processLockupTransfer

Process a transfer through lockup rules


```solidity
function processLockupTransfer(address from, address to, uint256 customLotId, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The sender address|
|`to`|`address`|The recipient address|
|`customLotId`|`uint256`|The custom lot ID|
|`amount`|`uint256`|The amount being transferred|


### migrateLockupLotState

Handle lot state migration during adjustments


```solidity
function migrateLockupLotState(bytes32 oldLotId, bytes32 newLotId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oldLotId`|`bytes32`|The old lot ID (hash-based)|
|`newLotId`|`bytes32`|The new lot ID (hash-based)|


### _getActiveLockupAmount

Calculate active lockup amount from an array of lockup periods


```solidity
function _getActiveLockupAmount(LockupPeriod[] storage lockups) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lockups`|`LockupPeriod[]`|Array of lockup periods|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Total amount currently locked|


### _autoReleaseExpiredLockups

Automatically release expired lockups


```solidity
function _autoReleaseExpiredLockups(uint256 customLotId, address account) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`customLotId`|`uint256`|The custom lot ID|
|`account`|`address`|The account address|


### validateTransfer

Validate if a transfer complies with lockup rules (IComplianceCondition interface)


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

Process a transfer through lockup rules (IComplianceCondition interface)


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
|`data`|`bytes`|Additional data (unused for lockups)|


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
### LotLockupCreated

```solidity
event LotLockupCreated(
    uint256 indexed customLotId, uint256 indexed lockupIndex, uint256 unlockTime, uint256 lockedAmount, string reason
);
```

### AccountLockupCreated

```solidity
event AccountLockupCreated(
    address indexed account, uint256 indexed lockupIndex, uint256 unlockTime, uint256 lockedAmount, string reason
);
```

### LockupReleased

```solidity
event LockupReleased(uint256 indexed customLotId, uint256 indexed lockupIndex, uint256 amount);
```

### AccountLockupReleased

```solidity
event AccountLockupReleased(address indexed account, uint256 indexed lockupIndex, uint256 amount);
```

## Errors
### Lockup_InvalidAmount

```solidity
error Lockup_InvalidAmount();
```

### Lockup_InvalidUnlockTime

```solidity
error Lockup_InvalidUnlockTime();
```

### Lockup_LockupNotFound

```solidity
error Lockup_LockupNotFound();
```

### Lockup_LockupNotExpired

```solidity
error Lockup_LockupNotExpired();
```

### Lockup_TransferViolatesLockup

```solidity
error Lockup_TransferViolatesLockup();
```

## Structs
### LockupPeriod

```solidity
struct LockupPeriod {
    uint256 unlockTime;
    uint256 lockedAmount;
    bool isActive;
    string reason;
}
```

### LockupStorage

```solidity
struct LockupStorage {
    mapping(uint256 => LockupPeriod[]) lotLockups;
    mapping(address => LockupPeriod[]) accountLockups;
    mapping(uint256 => uint256) totalLockedByLot;
    mapping(address => uint256) totalLockedByAccount;
}
```

