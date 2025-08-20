# DocumentRegistryDeploymentFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Documents/factory/facets/DocumentRegistryDeploymentFacet.sol)

**Inherits:**
[FactoryBase](/src/Diamonds/factory/FactoryBase.sol/abstract.FactoryBase.md)

Deployment facet for DocumentRegistry diamonds

*Handles the deployment logic for new DocumentRegistry instances*


## Functions
### constructor


```solidity
constructor(address _facetRegistry, address _globalAccessManager) FactoryBase(_facetRegistry, _globalAccessManager);
```

### deployDiamond

Deploy a new DocumentRegistry diamond (global registry)


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
|`registry`|`address`|Address of the deployed DocumentRegistry|


### deployDocumentRegistryForEntity

Deploy a new DocumentRegistry diamond for a specific entity


```solidity
function deployDocumentRegistryForEntity(
    IDiamond.FacetCut[] memory facetCuts,
    address entity,
    address identityAccessManager,
    string memory registryName,
    bytes32 salt
) external returns (address registry);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`facetCuts`|`IDiamond.FacetCut[]`|Array of facet cuts to install|
|`entity`|`address`|Address of the entity who will own this registry|
|`identityAccessManager`|`address`|Address of the entity's access manager|
|`registryName`|`string`|Name for the registry|
|`salt`|`bytes32`|Salt for CREATE2 deployment|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`registry`|`address`|Address of the deployed DocumentRegistry|


### _deployDocumentRegistry

*Internal function to deploy DocumentRegistry diamond*


```solidity
function _deployDocumentRegistry(
    IDiamond.FacetCut[] memory facetCuts,
    address globalAccessManager,
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
### DocumentRegistryDeployed

```solidity
event DocumentRegistryDeployed(
    address indexed registry, address indexed deployer, bool isGlobal, string registryName, address accessManager
);
```

