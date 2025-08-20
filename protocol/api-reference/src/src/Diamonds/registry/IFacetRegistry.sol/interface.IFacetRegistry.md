# IFacetRegistry
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/registry/IFacetRegistry.sol)

Interface of the FacetRegistry contract with enhanced features


## Functions
### addFacet

Registers a new facet.


```solidity
function addFacet(address facet, bytes4[] calldata selectors) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`facet`|`address`|Address of the facet to add.|
|`selectors`|`bytes4[]`|Function selectors of the facet.|


### removeFacet

Removes a facet from the registry.


```solidity
function removeFacet(address facet) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`facet`|`address`|Address of the facet to remove.|


### deployFacet

Deploys a new facet and registers it.


```solidity
function deployFacet(bytes32 salt, bytes calldata creationCode, bytes4[] calldata selectors)
    external
    returns (address facet);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`salt`|`bytes32`|Salt used to create the address of the new facet.|
|`creationCode`|`bytes`|Creation code of the new facet.|
|`selectors`|`bytes4[]`|Function selectors of the new facet.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`facet`|`address`|Address of the new facet.|


### computeFacetAddress

Computes the address of a facet deployed with the given salt and creation code.


```solidity
function computeFacetAddress(bytes32 salt, bytes calldata creationCode) external view returns (address facet);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`salt`|`bytes32`|Salt used to create the address of the new facet.|
|`creationCode`|`bytes`|Creation code of the new facet.|


### facetSelectors

Returns the selectors of a registered facet.


```solidity
function facetSelectors(address facet) external view returns (bytes4[] memory selectors);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`facet`|`address`|The address of the facet.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`selectors`|`bytes4[]`|The selectors of the facet.|


### facetAddresses

Returns the addresses of all registered facets.


```solidity
function facetAddresses() external view returns (address[] memory facets);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`facets`|`address[]`|The addresses of all registered facets.|


### registerFacetByName


```solidity
function registerFacetByName(
    string memory name,
    address facetAddress,
    bytes4[] memory selectors,
    string memory description,
    string memory category
) external;
```

### updateFacet


```solidity
function updateFacet(string memory name, address newFacetAddress) external;
```

### updateFacetWithSelectors


```solidity
function updateFacetWithSelectors(string memory name, address newFacetAddress, bytes4[] memory newSelectors) external;
```

### upsertFacet


```solidity
function upsertFacet(
    string memory name,
    address facetAddress,
    bytes4[] memory selectors,
    string memory description,
    string memory category
) external;
```

### deactivateFacet


```solidity
function deactivateFacet(string memory name) external;
```

### registerTemplate


```solidity
function registerTemplate(
    string memory name,
    string memory description,
    string[] memory requiredFacets,
    string[] memory optionalFacets,
    bytes memory initializationData
) external;
```

### updateTemplate


```solidity
function updateTemplate(
    string memory name,
    string memory description,
    string[] memory requiredFacets,
    string[] memory optionalFacets,
    bytes memory initializationData
) external;
```

### deactivateTemplate


```solidity
function deactivateTemplate(string memory name) external;
```

### buildFacetCutsFromTemplate


```solidity
function buildFacetCutsFromTemplate(string memory templateName)
    external
    view
    returns (IDiamond.FacetCut[] memory cuts);
```

### buildFacetCutsFromNames


```solidity
function buildFacetCutsFromNames(string[] memory facetNames) external view returns (IDiamond.FacetCut[] memory cuts);
```

### buildFacetCutsFromConfig


```solidity
function buildFacetCutsFromConfig(DeploymentConfig memory config)
    external
    view
    returns (IDiamond.FacetCut[] memory cuts);
```

### getFacetByName


```solidity
function getFacetByName(string memory name) external view returns (FacetInfo memory info);
```

### getFacetsByCategory


```solidity
function getFacetsByCategory(string memory category) external view returns (string[] memory facetNames);
```

### getTemplate


```solidity
function getTemplate(string memory name) external view returns (FacetTemplate memory template);
```

### getAllFacetNames


```solidity
function getAllFacetNames() external view returns (string[] memory names);
```

### getAllTemplateNames


```solidity
function getAllTemplateNames() external view returns (string[] memory names);
```

### getAllCategories


```solidity
function getAllCategories() external view returns (string[] memory categories);
```

## Structs
### FacetInfo

```solidity
struct FacetInfo {
    address facetAddress;
    bytes4[] selectors;
    string name;
    string description;
    string category;
    bool isActive;
    uint256 version;
}
```

### FacetTemplate

```solidity
struct FacetTemplate {
    string name;
    string description;
    string[] requiredFacets;
    string[] optionalFacets;
    bytes initializationData;
    bool isActive;
    uint256 version;
}
```

### DeploymentConfig

```solidity
struct DeploymentConfig {
    string templateName;
    string[] customFacets;
    string[] excludedFacets;
    bytes customInitData;
}
```

