# AttestationAdminFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Attestations/registry/facets/AttestationAdminFacet.sol)

**Inherits:**
[AccessManaged](/src/Authorization/AccessManaged.sol/abstract.AccessManaged.md)

*Facet for administrative functions in the AttestationRegistry diamond*


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

*Initialize the AttestationRegistry with EAS contracts (legacy - for global registry)*


```solidity
function initialize(address easAddress, address schemaRegistryAddress) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`easAddress`|`address`|Address of the EAS contract|
|`schemaRegistryAddress`|`address`|Address of the SchemaRegistry contract|


### initializeWithMetadata

*Initialize the AttestationRegistry with extended parameters (for factory deployment)*


```solidity
function initializeWithMetadata(
    address globalAccessManager,
    address easAddress,
    address schemaRegistryAddress,
    string memory registryName,
    bool isGlobal
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`globalAccessManager`|`address`|Address of the GlobalAccessManager contract|
|`easAddress`|`address`|Address of the EAS contract|
|`schemaRegistryAddress`|`address`|Address of the SchemaRegistry contract|
|`registryName`|`string`|Name of the registry|
|`isGlobal`|`bool`|Whether this is a global registry|


### initializeForEntity

*Initialize entity-specific registry with explicit entity and access manager*


```solidity
function initializeForEntity(
    address entity,
    address identityAccessManager,
    address easAddress,
    address schemaRegistryAddress,
    string memory registryName
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|Address of the entity who owns this registry|
|`identityAccessManager`|`address`|Address of the entity's access manager|
|`easAddress`|`address`|Address of the EAS contract|
|`schemaRegistryAddress`|`address`|Address of the SchemaRegistry contract|
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


### setEAS

*Set the EAS contract address*


```solidity
function setEAS(address easAddress) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`easAddress`|`address`|The new EAS contract address|


### setSchemaRegistry

*Set the SchemaRegistry contract address*


```solidity
function setSchemaRegistry(address schemaRegistryAddress) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`schemaRegistryAddress`|`address`|The new SchemaRegistry contract address|


### setValidAttestor

*Set valid attestor for an entity*


```solidity
function setValidAttestor(address attestor, bool isValid) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`attestor`|`address`|The attestor address|
|`isValid`|`bool`|Whether the attestor is valid|


### authorizeDelegator

*Authorize a delegator to submit delegated attestations*


```solidity
function authorizeDelegator(address delegator) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`delegator`|`address`|The delegator address|


### deauthorizeDelegator

*Deauthorize a delegator from submitting delegated attestations*


```solidity
function deauthorizeDelegator(address delegator) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`delegator`|`address`|The delegator address|


### isAuthorizedDelegator

*Check if a delegator is authorized*


```solidity
function isAuthorizedDelegator(address delegator) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`delegator`|`address`|The delegator address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if authorized|


### isAttestorTrustedByEntity

*Check if an attestor is trusted by an entity*


```solidity
function isAttestorTrustedByEntity(address entity, address attestor) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|The entity address|
|`attestor`|`address`|The attestor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if trusted|


### isGloballyAuthorizedAttestor

*Check if an attestor is globally authorized (has SYSTEM_ATTESTOR_ROLE)*


```solidity
function isGloballyAuthorizedAttestor(address attestor) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`attestor`|`address`|The attestor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if globally authorized|


### withdrawAttestationPayment

*Withdraw attestation payment from escrow*


```solidity
function withdrawAttestationPayment(address subject, bytes32 easUID) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subject`|`address`|The subject address|
|`easUID`|`bytes32`|The EAS schema UID|


### emergencyWithdraw

*Emergency function to withdraw all contract funds (admin only)*


```solidity
function emergencyWithdraw(address payable recipient) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`recipient`|`address payable`|The recipient address|


### schemaRegistry

*Get the SchemaRegistry contract address*


```solidity
function schemaRegistry() external view returns (ISchemaRegistry);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`ISchemaRegistry`|ISchemaRegistry The SchemaRegistry contract|


### getBalance

*Get contract balance*


```solidity
function getBalance() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 The contract balance in wei|


### pauseSchema

*Pause/unpause specific schema operations (admin only)*


```solidity
function pauseSchema(bytes32 easUID, bool paused) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`easUID`|`bytes32`|The schema UID|
|`paused`|`bool`|Whether to pause the schema|


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
### ValidAttestorSet

```solidity
event ValidAttestorSet(address indexed entity, address indexed attestor, bool isValid);
```

### EASUpdated

```solidity
event EASUpdated(address indexed oldEAS, address indexed newEAS);
```

### AttestationRegistryDeployed

```solidity
event AttestationRegistryDeployed(
    address indexed registryAddress, address admin, address easAddress, address schemaRegistryAddress
);
```

### DelegatorAuthorized

```solidity
event DelegatorAuthorized(address indexed delegator);
```

### DelegatorDeauthorized

```solidity
event DelegatorDeauthorized(address indexed delegator);
```

### SchemaUpdated

```solidity
event SchemaUpdated(bytes32 indexed easUID, string name, bool active);
```

