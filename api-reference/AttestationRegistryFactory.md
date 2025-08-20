# AttestationRegistryFactory
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Attestations/factory/AttestationRegistryFactory.sol)

**Inherits:**
[Factory](/src/Diamonds/factory/Factory.sol/contract.Factory.md)

Factory for deploying AttestationRegistry diamonds

*Enables both global and entity-specific attestation registries*


## Functions
### constructor

Deploy AttestationRegistryFactory


```solidity
constructor(address _facetRegistry, address _globalAccessManager)
    Factory("AttestationRegistryFactory", _facetRegistry, _globalAccessManager, "AttestationRegistryFactory");
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_facetRegistry`|`address`|Address of the FacetRegistry|
|`_globalAccessManager`|`address`|Address of the GlobalAccessManager|


### deployAttestationRegistry

Deploy a new AttestationRegistry diamond


```solidity
function deployAttestationRegistry(
    string memory registryName,
    address easAddress,
    address schemaRegistryAddress,
    bool isGlobal,
    bytes32 salt
) external returns (address registry);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`registryName`|`string`|Name for the registry (e.g., "GlobalRegistry", "AcmeCorpRegistry")|
|`easAddress`|`address`|Address of the EAS contract|
|`schemaRegistryAddress`|`address`|Address of the SchemaRegistry contract|
|`isGlobal`|`bool`|Whether this is a global registry (protocol-wide)|
|`salt`|`bytes32`|Salt for CREATE2 deployment|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`registry`|`address`|Address of the deployed AttestationRegistry|


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
### AttestationRegistryDeployed

```solidity
event AttestationRegistryDeployed(
    address indexed registry, address indexed deployer, bool isGlobal, string registryName
);
```

