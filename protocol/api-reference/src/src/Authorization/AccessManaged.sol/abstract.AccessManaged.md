# AccessManaged
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Authorization/AccessManaged.sol)

Helper contract for instance contracts to check roles against their IdentityAccessManager diamond

*Provides modifiers and helper functions for role-based access control*


## State Variables
### accessManager

```solidity
IAccessManager public immutable accessManager;
```


## Functions
### constructor

*Constructor*


```solidity
constructor(address _accessManager);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_accessManager`|`address`|The IdentityAccessManager diamond contract address|


### onlyRole

*Modifier to check if caller has a specific role*


```solidity
modifier onlyRole(uint64 role);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`role`|`uint64`|The role ID to check|


### onlyEntity

*Modifier to check if caller has the ENTITY_ROLE (which is ADMIN_ROLE)*


```solidity
modifier onlyEntity();
```

### onlyEntityOrAgent

*Modifier to check if caller has ENTITY_ROLE or PLACEMENT_AGENT_ROLE*


```solidity
modifier onlyEntityOrAgent();
```

### onlyAttestor

*Modifier to check if caller has the ATTESTOR_ROLE*


```solidity
modifier onlyAttestor();
```

### onlyTransferAgent

*Modifier to check if caller has the TRANSFER_AGENT_ROLE*


```solidity
modifier onlyTransferAgent();
```

### onlyComplianceOfficer

*Modifier to check if caller has the COMPLIANCE_OFFICER_ROLE*


```solidity
modifier onlyComplianceOfficer();
```

### _checkRole

*Internal function to check if an account has a specific role*


```solidity
function _checkRole(uint64 role, address account) internal view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`role`|`uint64`|The role ID to check|
|`account`|`address`|The account to check|


### _hasRole

*Internal function to check if an account has a specific role*


```solidity
function _hasRole(uint64 role, address account) internal view returns (bool accountHasRole);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`role`|`uint64`|The role ID to check|
|`account`|`address`|The account to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`accountHasRole`|`bool`|True if the account has the role|


### hasRole

*Check if an account has a specific role (external function)*


```solidity
function hasRole(uint64 role, address account) external view returns (bool rolePresent);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`role`|`uint64`|The role ID to check|
|`account`|`address`|The account to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`rolePresent`|`bool`|True if the account has the role|


### getEntity

*Get the entity address from the access manager diamond*


```solidity
function getEntity() external view returns (address entity);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|The entity address|


### _getEntityFromFacet

*Internal helper to call facet function (used in try/catch)*


```solidity
function _getEntityFromFacet() external view returns (address entity);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|The entity address from the facet|


### hasAnyEntityRole

*Check if an account has any entity-related role*


```solidity
function hasAnyEntityRole(address account) external view returns (bool hasAnyRole);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The account to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`hasAnyRole`|`bool`|True if the account has any entity role|


## Events
### AccessManagerSet

```solidity
event AccessManagerSet(address indexed accessManager);
```

