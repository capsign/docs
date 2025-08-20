# AttestationRegistryDeploymentFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Attestations/factory/facets/AttestationRegistryDeploymentFacet.sol)

**Inherits:**
[FactoryBase](/src/Diamonds/factory/FactoryBase.sol/abstract.FactoryBase.md)

Deployment facet for AttestationRegistry diamonds

*Handles the deployment logic for new AttestationRegistry instances*


## Functions
### constructor


```solidity
constructor(address _facetRegistry, address _globalAccessManager) FactoryBase(_facetRegistry, _globalAccessManager);
```

### deployDiamond

Deploy a new AttestationRegistry diamond (global registry)


```solidity
function deployDiamond(IDiamond.FacetCut[] memory facetCuts, bytes memory constructorArgs, bytes32 salt)
    external
    override
    returns (address registry);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`facetCuts`|`IDiamond.FacetCut[]`|Array of facet cuts to install|
|`constructorArgs`|`bytes`|ABI-encoded constructor arguments|
|`salt`|`bytes32`|Salt for CREATE2 deployment|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`registry`|`address`|Address of the deployed AttestationRegistry|


### deployEntityRegistry

Deploy a new AttestationRegistry diamond for a specific entity


```solidity
function deployEntityRegistry(
    IDiamond.FacetCut[] memory facetCuts,
    address entity,
    address identityAccessManager,
    address easAddress,
    address schemaRegistryAddress,
    string memory registryName,
    bytes32 salt
) external returns (address registry);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`facetCuts`|`IDiamond.FacetCut[]`|Array of facet cuts to install|
|`entity`|`address`|The entity address|
|`identityAccessManager`|`address`|The entity's access manager address|
|`easAddress`|`address`|Address of the EAS contract|
|`schemaRegistryAddress`|`address`|Address of the SchemaRegistry contract|
|`registryName`|`string`|Name of the registry|
|`salt`|`bytes32`|Salt for CREATE2 deployment|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`registry`|`address`|Address of the deployed AttestationRegistry|


### _deployAttestationRegistry

*Internal function to deploy AttestationRegistry diamond*


```solidity
function _deployAttestationRegistry(
    IDiamond.FacetCut[] memory facetCuts,
    address globalAccessManager,
    address easAddress,
    address schemaRegistryAddress,
    string memory registryName,
    bool isGlobal,
    address entity,
    address accessManager,
    bytes32 salt
) internal returns (address registry);
```

### _getInitializationFacet

*Get the initialization facet address from facet cuts*


```solidity
function _getInitializationFacet(IDiamond.FacetCut[] memory facetCuts) internal pure returns (address);
```

## Events
### AttestationRegistryDeployed

```solidity
event AttestationRegistryDeployed(
    address indexed registry,
    address indexed deployer,
    bool isGlobal,
    string registryName,
    address easAddress,
    address schemaRegistryAddress,
    address accessManager
);
```

