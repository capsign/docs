# IdentityAccessManagerRoleFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Authorization/iam/facets/IdentityAccessManagerRoleFacet.sol)

Allows entities to create, manage, and customize roles based on their needs

*Facet for dynamic role management in IdentityAccessManager*


## State Variables
### CUSTOM_ROLE_START

```solidity
uint64 public constant CUSTOM_ROLE_START = 1000;
```


## Functions
### createCustomRole

*Create a custom role for this entity*


```solidity
function createCustomRole(string calldata name, string calldata description, uint32 executionDelay)
    external
    returns (uint64 roleId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|The role name|
|`description`|`string`|The role description|
|`executionDelay`|`uint32`|The execution delay for this role|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`roleId`|`uint64`|The newly created role ID|


### updateCustomRole

*Update an existing custom role*


```solidity
function updateCustomRole(uint64 roleId, string calldata name, string calldata description, uint32 executionDelay)
    external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roleId`|`uint64`|The role ID to update|
|`name`|`string`|The new role name|
|`description`|`string`|The new role description|
|`executionDelay`|`uint32`|The new execution delay|


### deactivateCustomRole

*Deactivate a custom role (cannot be deleted due to AccessManager constraints)*


```solidity
function deactivateCustomRole(uint64 roleId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roleId`|`uint64`|The role ID to deactivate|


### getCustomRoles

*Get all custom roles for this entity*


```solidity
function getCustomRoles() external view returns (IdentityAccessManagerStorage.CustomRoleDefinition[] memory roles);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`roles`|`IdentityAccessManagerStorage.CustomRoleDefinition[]`|Array of custom role definitions|


### getCustomRole

*Get a specific custom role*


```solidity
function getCustomRole(uint64 roleId)
    external
    view
    returns (IdentityAccessManagerStorage.CustomRoleDefinition memory role);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roleId`|`uint64`|The role ID to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`role`|`IdentityAccessManagerStorage.CustomRoleDefinition`|The role definition|


### getEntityInfo

*Get entity information including ELF code*


```solidity
function getEntityInfo() external view returns (address entity, string memory entityName);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|The entity address|
|`entityName`|`string`|The entity name|


### _canManageEntity

*Check if caller can manage this entity*


```solidity
function _canManageEntity() internal view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if caller has management authority|


## Events
### CustomRoleCreated

```solidity
event CustomRoleCreated(uint64 indexed roleId, string name, address indexed createdBy);
```

### CustomRoleUpdated

```solidity
event CustomRoleUpdated(uint64 indexed roleId, string name);
```

### CustomRoleDeactivated

```solidity
event CustomRoleDeactivated(uint64 indexed roleId);
```

