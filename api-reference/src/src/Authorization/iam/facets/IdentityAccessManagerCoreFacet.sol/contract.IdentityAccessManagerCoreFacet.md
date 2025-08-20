# IdentityAccessManagerCoreFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Authorization/iam/facets/IdentityAccessManagerCoreFacet.sol)

*Core facet for IdentityAccessManager diamond that handles basic entity access management*


## State Variables
### ADMIN_ROLE

```solidity
uint64 public constant ADMIN_ROLE = 0;
```


### FINANCE_ROLE

```solidity
uint64 public constant FINANCE_ROLE = 100;
```


### GOVERNANCE_ROLE

```solidity
uint64 public constant GOVERNANCE_ROLE = 101;
```


### MEMBER_ROLE

```solidity
uint64 public constant MEMBER_ROLE = 102;
```


### AGENT_ROLE

```solidity
uint64 public constant AGENT_ROLE = 103;
```


### TIER_STARTER

```solidity
uint8 public constant TIER_STARTER = 0;
```


### TIER_SEED

```solidity
uint8 public constant TIER_SEED = 1;
```


### TIER_SCALE

```solidity
uint8 public constant TIER_SCALE = 2;
```


## Functions
### onlyEntityAdmin

*Modifier: caller must be entity, IAM admin, or protocol admin (via GAM)*


```solidity
modifier onlyEntityAdmin();
```

### initialize

*Initialize the IdentityAccessManager diamond*


```solidity
function initialize(address _entity, string calldata _entityName, address _globalAccessManager) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_entity`|`address`|The entity address|
|`_entityName`|`string`|Human-readable name for the entity|
|`_globalAccessManager`|`address`|The global access manager address|


### setGlobalAccessManager

*Set the global access manager reference*


```solidity
function setGlobalAccessManager(address _globalAccessManager) external onlyEntityAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_globalAccessManager`|`address`|Address of the global access manager|


### updateEntityName

*Update the entity name*


```solidity
function updateEntityName(string calldata newName) external onlyEntityAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newName`|`string`|The new entity name|


### addManagedAsset

*Add an asset to be managed by this access manager*


```solidity
function addManagedAsset(address asset) external onlyEntityAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset address|


### removeManagedAsset

*Remove an asset from management*


```solidity
function removeManagedAsset(address asset) external onlyEntityAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset address|


### getEntityRoles

*Get role information for UI/tooling*


```solidity
function getEntityRoles() external pure returns (uint64[] memory roleIds, string[] memory roleNames);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`roleIds`|`uint64[]`|Array of all predefined role IDs|
|`roleNames`|`string[]`|Array of corresponding role names|


### getEntityInfo

*Get entity information*


```solidity
function getEntityInfo()
    external
    view
    returns (address entity, string memory entityName, address globalAccessManager);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|The entity address|
|`entityName`|`string`|The entity name|
|`globalAccessManager`|`address`|The global access manager address|


### getManagedAssets

*Get managed assets*


```solidity
function getManagedAssets() external view returns (address[] memory assets);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`assets`|`address[]`|Array of managed asset addresses|


### isManagedAsset

*Check if an asset is managed*


```solidity
function isManagedAsset(address asset) external view returns (bool managed);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|The asset address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`managed`|`bool`|True if the asset is managed|


### _canManageGlobalSettings

*Check if caller can manage global settings*


```solidity
function _canManageGlobalSettings() internal view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if caller is entity admin or current global protocol admin|


### _canManageEntitySettings

*Check if caller can manage entity-level settings (admin/member management, metadata)*


```solidity
function _canManageEntitySettings() internal view returns (bool);
```

## Events
### IdentityAccessManagerCreated

```solidity
event IdentityAccessManagerCreated(address indexed entity, string entityName);
```

### EntityNameUpdated

```solidity
event EntityNameUpdated(string oldName, string newName);
```

### GlobalAccessManagerUpdated

```solidity
event GlobalAccessManagerUpdated(address indexed oldManager, address indexed newManager);
```

### AssetAdded

```solidity
event AssetAdded(address indexed asset);
```

### AssetRemoved

```solidity
event AssetRemoved(address indexed asset);
```

