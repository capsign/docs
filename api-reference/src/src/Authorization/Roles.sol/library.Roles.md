# Roles
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Authorization/Roles.sol)

Library containing all protocol role definitions

*Centralizes role constants for consistency across the protocol*


## State Variables
### ADMIN_ROLE

```solidity
uint64 public constant ADMIN_ROLE = 0;
```


### PROTOCOL_ADMIN_ROLE

```solidity
uint64 public constant PROTOCOL_ADMIN_ROLE = 1;
```


### REGISTRY_ADMIN_ROLE

```solidity
uint64 public constant REGISTRY_ADMIN_ROLE = 2;
```


### FACTORY_ADMIN_ROLE

```solidity
uint64 public constant FACTORY_ADMIN_ROLE = 3;
```


### SYSTEM_ATTESTOR_ROLE

```solidity
uint64 public constant SYSTEM_ATTESTOR_ROLE = 4;
```


### EMERGENCY_ADMIN_ROLE

```solidity
uint64 public constant EMERGENCY_ADMIN_ROLE = 5;
```


### ENTITY_ROLE

```solidity
uint64 public constant ENTITY_ROLE = 6;
```


### BOARD_GOVERNANCE_ROLE

```solidity
uint64 public constant BOARD_GOVERNANCE_ROLE = 1;
```


### TOKENHOLDER_GOVERNANCE_ROLE

```solidity
uint64 public constant TOKENHOLDER_GOVERNANCE_ROLE = 2;
```


### DIRECTOR_ROLE

```solidity
uint64 public constant DIRECTOR_ROLE = 3;
```


### CHIEF_EXECUTIVE_OFFICER_ROLE

```solidity
uint64 public constant CHIEF_EXECUTIVE_OFFICER_ROLE = 4;
```


### CHIEF_FINANCIAL_OFFICER_ROLE

```solidity
uint64 public constant CHIEF_FINANCIAL_OFFICER_ROLE = 5;
```


### CHIEF_COMPLIANCE_OFFICER_ROLE

```solidity
uint64 public constant CHIEF_COMPLIANCE_OFFICER_ROLE = 6;
```


### PRESIDENT_ROLE

```solidity
uint64 public constant PRESIDENT_ROLE = 7;
```


### SECRETARY_ROLE

```solidity
uint64 public constant SECRETARY_ROLE = 8;
```


### TREASURER_ROLE

```solidity
uint64 public constant TREASURER_ROLE = 9;
```


### EMPLOYEE_ROLE

```solidity
uint64 public constant EMPLOYEE_ROLE = 21;
```


### ADVISOR_ROLE

```solidity
uint64 public constant ADVISOR_ROLE = 22;
```


### INTERNAL_ATTESTOR_ROLE

```solidity
uint64 public constant INTERNAL_ATTESTOR_ROLE = 23;
```


### INTERNAL_COMPLIANCE_OFFICER_ROLE

```solidity
uint64 public constant INTERNAL_COMPLIANCE_OFFICER_ROLE = 24;
```


### PLACEMENT_AGENT_ROLE

```solidity
uint64 public constant PLACEMENT_AGENT_ROLE = 41;
```


### UNDERWRITER_ROLE

```solidity
uint64 public constant UNDERWRITER_ROLE = 42;
```


### TRANSFER_AGENT_ROLE

```solidity
uint64 public constant TRANSFER_AGENT_ROLE = 43;
```


### OFFERING_OPERATOR_ROLE

```solidity
uint64 public constant OFFERING_OPERATOR_ROLE = 44;
```


### EXTERNAL_COMPLIANCE_SERVICE_ROLE

```solidity
uint64 public constant EXTERNAL_COMPLIANCE_SERVICE_ROLE = 45;
```


### ACCREDITATION_AGENT_ROLE

```solidity
uint64 public constant ACCREDITATION_AGENT_ROLE = 46;
```


### ATTORNEY_ROLE

```solidity
uint64 public constant ATTORNEY_ROLE = 47;
```


### ENTITY_ADMIN_ROLE

```solidity
uint64 public constant ENTITY_ADMIN_ROLE = BOARD_GOVERNANCE_ROLE;
```


### ATTESTOR_ROLE

```solidity
uint64 public constant ATTESTOR_ROLE = INTERNAL_ATTESTOR_ROLE;
```


### COMPLIANCE_OFFICER_ROLE

```solidity
uint64 public constant COMPLIANCE_OFFICER_ROLE = EXTERNAL_COMPLIANCE_SERVICE_ROLE;
```


### CRITICAL_DELAY

```solidity
uint32 public constant CRITICAL_DELAY = 2 days;
```


### STANDARD_DELAY

```solidity
uint32 public constant STANDARD_DELAY = 1 days;
```


### SHORT_DELAY

```solidity
uint32 public constant SHORT_DELAY = 6 hours;
```


### IMMEDIATE

```solidity
uint32 public constant IMMEDIATE = 0;
```


## Functions
### getGlobalRoleIds

*Get all global protocol role IDs*


```solidity
function getGlobalRoleIds() internal pure returns (uint64[] memory roleIds);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`roleIds`|`uint64[]`|Array of all global protocol role IDs|


### getEntityRoleIds

*Get all entity-specific role IDs (organized by hierarchy)*


```solidity
function getEntityRoleIds() internal pure returns (uint64[] memory roleIds);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`roleIds`|`uint64[]`|Array of all entity-specific role IDs|


### getGlobalRoleName

*Get role name for a given global protocol role ID*


```solidity
function getGlobalRoleName(uint64 roleId) internal pure returns (string memory roleName);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roleId`|`uint64`|The role ID to get the name for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`roleName`|`string`|The human-readable role name|


### getEntityRoleName

*Get role name for a given entity-specific role ID*


```solidity
function getEntityRoleName(uint64 roleId) internal pure returns (string memory roleName);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roleId`|`uint64`|The role ID to get the name for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`roleName`|`string`|The human-readable role name|


### getGlobalRoleDelay

*Get execution delay for a given global protocol role ID*


```solidity
function getGlobalRoleDelay(uint64 roleId) internal pure returns (uint32 delay);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roleId`|`uint64`|The role ID to get the delay for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`delay`|`uint32`|The execution delay in seconds|


### getEntityRoleDelay

*Get execution delay for entity-specific roles (mostly immediate for business agility)*


```solidity
function getEntityRoleDelay(uint64 roleId) internal pure returns (uint32 delay);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roleId`|`uint64`|The role ID to get the delay for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`delay`|`uint32`|The execution delay in seconds|


