# GlobalAccessManagerEmergencyFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Authorization/gam/facets/GlobalAccessManagerEmergencyFacet.sol)

*Emergency controls facet for GlobalAccessManager diamond*


## Functions
### activateEmergencyMode

*Activate emergency mode*

*Only EMERGENCY_ADMIN_ROLE can call this*


```solidity
function activateEmergencyMode() external;
```

### deactivateEmergencyMode

*Deactivate emergency mode*

*Only EMERGENCY_ADMIN_ROLE can call this*


```solidity
function deactivateEmergencyMode() external;
```

### emergencyExecute

*Emergency function to execute critical operations immediately*

*Only EMERGENCY_ADMIN_ROLE can call this*


```solidity
function emergencyExecute(address target, bytes calldata data) external returns (bytes memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`target`|`address`|The target contract|
|`data`|`bytes`|The function call data|


### configureEmergencyFunction

*Configure which functions can be called in emergency mode*

*Only PROTOCOL_ADMIN_ROLE can call this*


```solidity
function configureEmergencyFunction(bytes4 selector, bool allowed) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`selector`|`bytes4`|The function selector|
|`allowed`|`bool`|Whether the function is allowed in emergency|


### batchConfigureEmergencyFunctions

*Batch configure emergency functions*

*Only PROTOCOL_ADMIN_ROLE can call this*


```solidity
function batchConfigureEmergencyFunctions(bytes4[] calldata selectors, bool[] calldata allowed) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`selectors`|`bytes4[]`|Array of function selectors|
|`allowed`|`bool[]`|Array of allowed flags|


### isEmergencyModeActive

*Check if emergency mode is active*


```solidity
function isEmergencyModeActive() external view returns (bool active, uint256 activatedAt);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`active`|`bool`|True if emergency mode is active|
|`activatedAt`|`uint256`|Timestamp when emergency mode was activated (0 if not active)|


### isEmergencyFunctionAllowed

*Check if a function is allowed in emergency mode*


```solidity
function isEmergencyFunctionAllowed(bytes4 selector) external view returns (bool allowed);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`selector`|`bytes4`|The function selector|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`allowed`|`bool`|True if the function is allowed in emergency|


### getEmergencyActionCount

*Get emergency action count for an address*


```solidity
function getEmergencyActionCount(address executor) external view returns (uint256 count);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`executor`|`address`|The executor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`count`|`uint256`|Number of emergency actions executed|


## Events
### EmergencyModeActivated

```solidity
event EmergencyModeActivated(address indexed activator, uint256 timestamp);
```

### EmergencyModeDeactivated

```solidity
event EmergencyModeDeactivated(address indexed deactivator, uint256 timestamp);
```

### EmergencyActionExecuted

```solidity
event EmergencyActionExecuted(address indexed executor, address indexed target, bytes4 indexed selector);
```

### EmergencyFunctionConfigured

```solidity
event EmergencyFunctionConfigured(bytes4 indexed selector, bool allowed);
```

