# IAccessControl
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/facets/access/IAccessControl.sol)

Unified interface for access control functionality across all factory diamonds

*Provides standardized access control functions that all factories should implement*


## Functions
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
function updateAccessManager(address newManager) external;
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


### pause

Pause the factory, preventing new deployments


```solidity
function pause() external;
```

### unpause

Unpause the factory, allowing new deployments


```solidity
function unpause() external;
```

### paused

Check if the factory is paused


```solidity
function paused() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the factory is paused|


### emergencyStop

Activate emergency stop, halting all operations


```solidity
function emergencyStop() external;
```

### liftEmergencyStop

Lift emergency stop, resuming normal operations


```solidity
function liftEmergencyStop() external;
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


### requireRestricted

Require that the caller is authorized

*This is exposed as a function so other facets can use it*


```solidity
function requireRestricted() external view;
```

### requireNotEmergencyStopped

Require that emergency stop is not active

*This is exposed as a function so other facets can use it*


```solidity
function requireNotEmergencyStopped() external view;
```

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

