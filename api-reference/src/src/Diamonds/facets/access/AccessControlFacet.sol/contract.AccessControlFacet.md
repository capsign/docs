# AccessControlFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/facets/access/AccessControlFacet.sol)

**Inherits:**
[AccessManaged](/src/Authorization/AccessManaged.sol/abstract.AccessManaged.md), Pausable

Unified access control facet for all factory diamonds

*Integrates with OpenZeppelin's AccessManager system and provides pausable functionality
This replaces the old custom role-based system with a standardized approach*


## State Variables
### ACCESS_CONTROL_STORAGE_SLOT

```solidity
bytes32 private constant ACCESS_CONTROL_STORAGE_SLOT = keccak256("diamonds.accesscontrol.storage");
```


## Functions
### constructor

Constructor for AccessControlFacet

*AccessManaged requires an initial authority, but in diamond pattern we initialize later*


```solidity
constructor() AccessManaged(address(0));
```

### initializeAccessControl

Initialize the access control with a global access manager


```solidity
function initializeAccessControl(address globalAccessManager) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`globalAccessManager`|`address`|The address of the global access manager|


### updateAccessManager

Update the access manager


```solidity
function updateAccessManager(address newManager) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newManager`|`address`|The new access manager address|


### getAccessManager

Get the current access manager


```solidity
function getAccessManager() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The address of the current access manager|


### pause

Pause the factory, preventing new deployments

*Only authorized accounts can pause*


```solidity
function pause() external restricted;
```

### unpause

Unpause the factory, allowing new deployments

*Only authorized accounts can unpause*


```solidity
function unpause() external restricted;
```

### emergencyStop

Activate emergency stop, halting all operations

*Only authorized accounts can activate emergency stop*


```solidity
function emergencyStop() external restricted;
```

### liftEmergencyStop

Lift emergency stop, resuming normal operations

*Only authorized accounts can lift emergency stop*


```solidity
function liftEmergencyStop() external restricted;
```

### emergencyStoppedState

Check if emergency stop is active


```solidity
function emergencyStoppedState() external view returns (bool isEmergencyStopped);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`isEmergencyStopped`|`bool`|True if emergency stop is active|


### hasRole

Check if an address has a specific role


```solidity
function hasRole(address account, uint64 role) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address to check|
|`role`|`uint64`|The role to check for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the account has the role|


### isAuthorized

Check if the caller is authorized for a specific function


```solidity
function isAuthorized(bytes4 functionSig) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`functionSig`|`bytes4`|The function signature to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the caller is authorized|


### requireRestricted

Modifier to restrict function access

*This is exposed as a function so other facets can use it*


```solidity
function requireRestricted() external view;
```

### requireNotEmergencyStopped

Modifier to check if emergency stop is not active

*This is exposed as a function so other facets can use it*


```solidity
function requireNotEmergencyStopped() external view;
```

### _hasRole

Internal function to check if an address has a role


```solidity
function _hasRole(address account, uint64 role) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address to check|
|`role`|`uint64`|The role to check for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the account has the role|


### _isAuthorized

Internal function to check if the caller is authorized


```solidity
function _isAuthorized(bytes4 functionSig) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`functionSig`|`bytes4`|The function signature to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the caller is authorized|


### _getAccessControlStorage

Get the storage for this facet


```solidity
function _getAccessControlStorage() internal pure returns (AccessControlStorage storage s);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`s`|`AccessControlStorage`|The storage struct|


## Events
### AccessManagerUpdated

```solidity
event AccessManagerUpdated(address indexed oldManager, address indexed newManager);
```

### EmergencyStop

```solidity
event EmergencyStop(address indexed account);
```

### EmergencyStopLifted

```solidity
event EmergencyStopLifted(address indexed account);
```

## Errors
### AccessControl_InvalidManager

```solidity
error AccessControl_InvalidManager();
```

### AccessControl_Unauthorized

```solidity
error AccessControl_Unauthorized();
```

### EmergencyStopActive

```solidity
error EmergencyStopActive();
```

## Structs
### AccessControlStorage

```solidity
struct AccessControlStorage {
    bool emergencyStopped;
}
```

