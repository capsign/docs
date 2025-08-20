# DocumentRegistryFactory
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Documents/factory/DocumentRegistryFactory.sol)

**Inherits:**
[Factory](/src/Diamonds/factory/Factory.sol/contract.Factory.md)

Factory for deploying DocumentRegistry diamonds

*Enables both global and entity-specific document registries*


## Functions
### constructor

Deploy DocumentRegistryFactory


```solidity
constructor(address _facetRegistry, address _globalAccessManager)
    Factory("DocumentRegistryFactory", _facetRegistry, _globalAccessManager, "DocumentRegistryFactory");
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_facetRegistry`|`address`|Address of the FacetRegistry|
|`_globalAccessManager`|`address`|Address of the GlobalAccessManager|


### deployDocumentRegistry

Deploy a new DocumentRegistry diamond


```solidity
function deployDocumentRegistry(string memory registryName, bool isGlobal, bytes32 salt)
    external
    returns (address registry);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`registryName`|`string`|Name for the registry (e.g., "GlobalDocuments", "AcmeCorpDocuments")|
|`isGlobal`|`bool`|Whether this is a global registry (protocol-wide)|
|`salt`|`bytes32`|Salt for CREATE2 deployment|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`registry`|`address`|Address of the deployed DocumentRegistry|


### deployDocumentRegistryForEntity

Deploy a new DocumentRegistry diamond for a specific entity


```solidity
function deployDocumentRegistryForEntity(
    address entity,
    address identityAccessManager,
    string memory registryName,
    bytes32 salt
) external returns (address registry);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|Address of the entity who will own this registry|
|`identityAccessManager`|`address`|Address of the entity's access manager|
|`registryName`|`string`|Name for the registry|
|`salt`|`bytes32`|Salt for CREATE2 deployment|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`registry`|`address`|Address of the deployed DocumentRegistry|


### getRegistryAddress

Get the address of a registry deployment


```solidity
function getRegistryAddress(string memory registryName, address deployer, bytes32 salt)
    external
    view
    returns (address registry);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`registryName`|`string`|Name of the registry|
|`deployer`|`address`|Address of the deployer|
|`salt`|`bytes32`|Salt used for deployment|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`registry`|`address`|Predicted address of the registry|


## Events
### DocumentRegistryDeployed

```solidity
event DocumentRegistryDeployed(address indexed registry, address indexed deployer, bool isGlobal, string registryName);
```

