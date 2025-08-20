# DocumentAdminFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Documents/registry/facets/DocumentAdminFacet.sol)

**Inherits:**
[AccessManaged](/src/Authorization/AccessManaged.sol/abstract.AccessManaged.md)

*Facet for administrative functions in the DocumentRegistry diamond*


## Functions
### constructor

*Constructor*


```solidity
constructor(address _globalAccessManager) AccessManaged(_globalAccessManager);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_globalAccessManager`|`address`|Address of the GlobalAccessManager contract|


### initialize

*Initialize the DocumentRegistry (legacy - for global registry)*


```solidity
function initialize(address owner) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|


### initializeWithMetadata

*Initialize the DocumentRegistry with extended parameters (for factory deployment)*


```solidity
function initializeWithMetadata(address globalAccessManager, string memory registryName, bool isGlobal) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`globalAccessManager`|`address`|Address of the GlobalAccessManager contract|
|`registryName`|`string`|Name of the registry|
|`isGlobal`|`bool`|Whether this is a global registry|


### initializeForEntity

*Initialize entity-specific registry with explicit entity and access manager*


```solidity
function initializeForEntity(address entity, address identityAccessManager, string memory registryName) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|Address of the entity who owns this registry|
|`identityAccessManager`|`address`|Address of the entity's access manager|
|`registryName`|`string`|Name of the registry|


### _getIdentityAccessManager

*Get the entity access manager for a given entity*


```solidity
function _getIdentityAccessManager(address entity, address globalAccessManager) internal view returns (address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|The entity address|
|`globalAccessManager`|`address`|The global access manager address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The entity's access manager address|


### _getIdentityAccessManagerFromFactory

*External helper function to get entity access manager (used in try/catch)*


```solidity
function _getIdentityAccessManagerFromFactory(address entity, address globalAccessManager)
    external
    view
    returns (address accessManager);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|The entity address|
|`globalAccessManager`|`address`|The global access manager address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`accessManager`|`address`|The entity's access manager address|


### getAccessManager

*Get the effective access manager for this registry*


```solidity
function getAccessManager() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The access manager address|


### getRegistryInfo

*Get registry information*


```solidity
function getRegistryInfo()
    external
    view
    returns (string memory registryName, bool isGlobal, address entity, address accessManager);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`registryName`|`string`|Name of the registry|
|`isGlobal`|`bool`|Whether this is a global registry|
|`entity`|`address`|The entity address (zero for global)|
|`accessManager`|`address`|The access manager address|


### setRegistryAdmin

*Set registry admin status*


```solidity
function setRegistryAdmin(address admin, bool isAdmin) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`admin`|`address`|The admin address|
|`isAdmin`|`bool`|Whether the address is an admin|


### isRegistryAdmin

*Check if an address is a registry admin*


```solidity
function isRegistryAdmin(address admin) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`admin`|`address`|The address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if the address is a registry admin|


### setRegistrationPaused

*Set registration paused status*


```solidity
function setRegistrationPaused(bool paused) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`paused`|`bool`|Whether registration is paused|


### isRegistrationPaused

*Check if registration is paused*


```solidity
function isRegistrationPaused() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if registration is paused|


### setMaxDocumentSize

*Set maximum document size*


```solidity
function setMaxDocumentSize(uint256 maxSize) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`maxSize`|`uint256`|The maximum document size in bytes (0 for no limit)|


### getMaxDocumentSize

*Get maximum document size*


```solidity
function getMaxDocumentSize() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 The maximum document size in bytes (0 for no limit)|


### setDefaultExpirationPeriod

*Set default expiration period*


```solidity
function setDefaultExpirationPeriod(uint256 period) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`period`|`uint256`|The default expiration period in seconds (0 for no expiration)|


### getDefaultExpirationPeriod

*Get default expiration period*


```solidity
function getDefaultExpirationPeriod() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 The default expiration period in seconds (0 for no expiration)|


### emergencyWithdraw

*Emergency function to withdraw all contract funds (admin only)*


```solidity
function emergencyWithdraw(address payable recipient) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`recipient`|`address payable`|The recipient address|


### getBalance

*Get contract balance*


```solidity
function getBalance() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 The contract balance in wei|


### _hasAdminRole

*Check if caller has admin role in the appropriate access manager*


```solidity
function _hasAdminRole(address account) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The account to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if has admin role|


### receive

*Allow contract to receive ETH*


```solidity
receive() external payable;
```

## Events
### DocumentRegistryCreated

```solidity
event DocumentRegistryCreated(address indexed registry);
```

### RegistryAdminSet

```solidity
event RegistryAdminSet(address indexed admin, bool isAdmin);
```

### RegistrationPausedSet

```solidity
event RegistrationPausedSet(bool paused);
```

### MaxDocumentSizeSet

```solidity
event MaxDocumentSizeSet(uint256 maxSize);
```

### DefaultExpirationPeriodSet

```solidity
event DefaultExpirationPeriodSet(uint256 period);
```

