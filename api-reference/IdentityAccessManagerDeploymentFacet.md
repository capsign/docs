# IdentityAccessManagerDeploymentFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Authorization/factory/facets/IdentityAccessManagerDeploymentFacet.sol)

Facet providing entity access manager deployment functionality for IdentityAccessManagerFactory diamond

*Handles the creation and initialization of IdentityAccessManager instances using FactoryBase*


## State Variables
### ENTITY_ACCESS_MANAGER_DEPLOYMENT_STORAGE_SLOT

```solidity
bytes32 private constant ENTITY_ACCESS_MANAGER_DEPLOYMENT_STORAGE_SLOT =
    keccak256("entity.access.manager.deployment.storage");
```


## Functions
### _getIdentityAccessManagerDeploymentStorage

Get the storage struct for this facet


```solidity
function _getIdentityAccessManagerDeploymentStorage()
    internal
    pure
    returns (IdentityAccessManagerDeploymentStorage storage ds);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`ds`|`IdentityAccessManagerDeploymentStorage`|The storage struct|


### _checkAccessManagerCreationAllowed

Check if access manager creation is allowed based on config


```solidity
function _checkAccessManagerCreationAllowed(address entity) internal view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|Entity creating the access manager|


### _checkAccessManagerLimits

Check access manager creation limits based on config


```solidity
function _checkAccessManagerLimits(address entity) internal view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|Entity creating the access manager|


### _checkAndCollectFee

Check and collect creation fee if required


```solidity
function _checkAndCollectFee() internal;
```

### _checkKYCRequirements

Check KYC requirements based on config


```solidity
function _checkKYCRequirements(address entity) internal view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|Entity creating the access manager|


### createAccessManager

Create an entity access manager

*The IAM contract address becomes the entity's on-chain identity*


```solidity
function createAccessManager() external payable returns (address accessManagerAddress);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`accessManagerAddress`|`address`|Address of the created access manager (entity identity)|


### createAccessManagerWithTemplate

Create an access manager using a specific template


```solidity
function createAccessManagerWithTemplate(string memory templateName)
    external
    payable
    returns (address accessManagerAddress);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`templateName`|`string`|Name of the template to use|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`accessManagerAddress`|`address`|Address of the created access manager|


### getAccessManager

Get access manager for an entity


```solidity
function getAccessManager(address entity) external view returns (address accessManager);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|The entity address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`accessManager`|`address`|The access manager address (zero if not exists)|


### getEntityForAccessManager

Get entity for an access manager


```solidity
function getEntityForAccessManager(address accessManager) external view returns (address entity);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`accessManager`|`address`|The access manager address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|The entity address (zero if not exists)|


### isAccessManager

Check if an address is a deployed access manager


```solidity
function isAccessManager(address accessManager) external view returns (bool isValid);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`accessManager`|`address`|The address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`isValid`|`bool`|True if it's a deployed access manager|


### getAccessManagerCount

Get access manager count


```solidity
function getAccessManagerCount() external view returns (uint256 count);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`count`|`uint256`|Total number of deployed access managers|


### getAccessManagerByIndex

Get access manager info by index


```solidity
function getAccessManagerByIndex(uint256 index) external view returns (IdentityAccessManagerInfo memory info);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`index`|`uint256`|The index to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`info`|`IdentityAccessManagerInfo`|The access manager info|


### hasAccessManager

Check if entity has an access manager


```solidity
function hasAccessManager(address entity) external view returns (bool hasManager);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|The entity address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`hasManager`|`bool`|True if entity has an access manager|


### getAccessManagerAddress

Get predicted address for access manager deployment


```solidity
function getAccessManagerAddress(address creator) external view returns (address predictedAddress);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`creator`|`address`|The creator (authorized representative) address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`predictedAddress`|`address`|The predicted deployment address|


### _validateAccessManagerParams

Validate access manager creation parameters


```solidity
function _validateAccessManagerParams(address entity) internal pure;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|The entity address|


### _prepareAccessManagerConstructorArgs

Prepare constructor arguments for access manager deployment


```solidity
function _prepareAccessManagerConstructorArgs() internal view returns (bytes memory constructorArgs);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`constructorArgs`|`bytes`|ABI-encoded constructor arguments|


### _recordAccessManagerCreation

Record access manager creation in storage


```solidity
function _recordAccessManagerCreation(address accessManagerAddress, address) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`accessManagerAddress`|`address`|The deployed access manager address|
|`<none>`|`address`||


### createAccessManager

*Deploy a new IdentityAccessManager for the caller (self-service)*

*IAM is deployed nameless - name should be assigned later via attestation*


```solidity
function createAccessManager(bytes32 salt) external returns (address accessManager);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`salt`|`bytes32`|Optional salt for deterministic deployment (use bytes32(0) for random)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`accessManager`|`address`|The address of the deployed access manager|


### _getFeeRecipient

Get fee recipient address from factory storage


```solidity
function _getFeeRecipient() internal view returns (address feeRecipient);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`feeRecipient`|`address`|The fee recipient address|


## Events
### AccessManagerCreated

```solidity
event AccessManagerCreated(address indexed accessManagerAddress, address indexed entity);
```

### AccessManagerCreationFeeCollected

```solidity
event AccessManagerCreationFeeCollected(address indexed entity, uint256 fee);
```

## Errors
### AccessManagerDeploymentFailed

```solidity
error AccessManagerDeploymentFailed();
```

### DeploymentFailed

```solidity
error DeploymentFailed();
```

### AccessManagerAlreadyExists

```solidity
error AccessManagerAlreadyExists(address entity);
```

### MaxAccessManagersExceeded

```solidity
error MaxAccessManagersExceeded(uint256 current, uint256 max);
```

### InsufficientFee

```solidity
error InsufficientFee(uint256 required, uint256 provided);
```

### KYCRequired

```solidity
error KYCRequired();
```

### TierUpgradeRequired

```solidity
error TierUpgradeRequired(string feature, uint8 currentTier, uint8 requiredTier);
```

### SubscriptionRequired

```solidity
error SubscriptionRequired(string feature);
```

### InvalidEntity

```solidity
error InvalidEntity();
```

## Structs
### IdentityAccessManagerDeploymentStorage

```solidity
struct IdentityAccessManagerDeploymentStorage {
    mapping(uint256 => IdentityAccessManagerInfo) accessManagers;
    uint256 accessManagerCount;
    mapping(address => address) entityToAccessManager;
    mapping(address => address) accessManagerToEntity;
    mapping(address => bool) isAccessManager;
}
```

### IdentityAccessManagerInfo

```solidity
struct IdentityAccessManagerInfo {
    address accessManagerAddress;
    address entity;
    uint256 createdAt;
    string entityName;
}
```

