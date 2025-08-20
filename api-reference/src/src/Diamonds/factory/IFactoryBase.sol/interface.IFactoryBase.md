# IFactoryBase
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/factory/IFactoryBase.sol)

Interface for all diamond factories with dynamic facet composition

*Defines common functionality for deploying diamonds with configurable facets*


## Functions
### deployWithTemplate

Deploy a diamond using a predefined template


```solidity
function deployWithTemplate(string memory templateName, bytes memory constructorArgs, bytes32 salt)
    external
    returns (address diamond);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`templateName`|`string`|Name of the template to use|
|`constructorArgs`|`bytes`|ABI-encoded constructor arguments|
|`salt`|`bytes32`|Salt for CREATE2 deployment (optional, uses default if empty)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`diamond`|`address`|Address of the deployed diamond|


### deployWithCustomFacets

Deploy a diamond with custom facet selection


```solidity
function deployWithCustomFacets(string[] memory facetNames, bytes memory constructorArgs, bytes32 salt)
    external
    returns (address diamond);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`facetNames`|`string[]`|Array of facet names to include|
|`constructorArgs`|`bytes`|ABI-encoded constructor arguments|
|`salt`|`bytes32`|Salt for CREATE2 deployment (optional, uses default if empty)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`diamond`|`address`|Address of the deployed diamond|


### deployWithConfig

Deploy a diamond with advanced configuration


```solidity
function deployWithConfig(IFacetRegistry.DeploymentConfig memory config, bytes memory constructorArgs, bytes32 salt)
    external
    returns (address diamond);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`config`|`IFacetRegistry.DeploymentConfig`|Deployment configuration with template and customizations|
|`constructorArgs`|`bytes`|ABI-encoded constructor arguments|
|`salt`|`bytes32`|Salt for CREATE2 deployment (optional, uses default if empty)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`diamond`|`address`|Address of the deployed diamond|


### registerTemplate

Register a new template (restricted to admins)


```solidity
function registerTemplate(
    string memory name,
    string memory description,
    string[] memory requiredFacets,
    string[] memory optionalFacets,
    bytes memory initializationData
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|Template name|
|`description`|`string`|Template description|
|`requiredFacets`|`string[]`|Required facet names|
|`optionalFacets`|`string[]`|Optional facet names|
|`initializationData`|`bytes`|Default initialization data|


### updateTemplate

Update an existing template (restricted to admins)


```solidity
function updateTemplate(
    string memory name,
    string memory description,
    string[] memory requiredFacets,
    string[] memory optionalFacets,
    bytes memory initializationData
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|Template name to update|
|`description`|`string`|New description|
|`requiredFacets`|`string[]`|New required facets|
|`optionalFacets`|`string[]`|New optional facets|
|`initializationData`|`bytes`|New initialization data|


### deactivateTemplate

Deactivate a template (restricted to admins)


```solidity
function deactivateTemplate(string memory name) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|Template name to deactivate|


### getAvailableTemplates

Get available templates


```solidity
function getAvailableTemplates() external view returns (string[] memory templateNames);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`templateNames`|`string[]`|Array of all template names|


### getTemplate

Get template information


```solidity
function getTemplate(string memory name) external view returns (IFacetRegistry.FacetTemplate memory template);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|Template name|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`template`|`IFacetRegistry.FacetTemplate`|Complete template information|


### getFacetsByCategory

Get available facets by category


```solidity
function getFacetsByCategory(string memory category) external view returns (string[] memory facetNames);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`category`|`string`|Facet category|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`facetNames`|`string[]`|Array of facet names in the category|


### getAllAvailableFacets

Get all available facets


```solidity
function getAllAvailableFacets() external view returns (string[] memory facetNames);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`facetNames`|`string[]`|Array of all facet names|


### getAllCategories

Get all categories


```solidity
function getAllCategories() external view returns (string[] memory categories);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`categories`|`string[]`|Array of all category names|


### previewTemplateFacets

Preview facet cuts for a template


```solidity
function previewTemplateFacets(string memory templateName) external view returns (IDiamond.FacetCut[] memory cuts);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`templateName`|`string`|Template name|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`cuts`|`IDiamond.FacetCut[]`|Array of facet cuts that would be used|


### previewCustomFacets

Preview facet cuts for custom facets


```solidity
function previewCustomFacets(string[] memory facetNames) external view returns (IDiamond.FacetCut[] memory cuts);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`facetNames`|`string[]`|Array of facet names|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`cuts`|`IDiamond.FacetCut[]`|Array of facet cuts that would be used|


### predictDiamondAddress

Predict the address of a diamond deployment


```solidity
function predictDiamondAddress(IDiamond.FacetCut[] memory facetCuts, bytes memory constructorArgs, bytes32 salt)
    external
    view
    returns (address predictedAddress);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`facetCuts`|`IDiamond.FacetCut[]`|Array of facet cuts|
|`constructorArgs`|`bytes`|Constructor arguments|
|`salt`|`bytes32`|Salt for CREATE2|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`predictedAddress`|`address`|The predicted address|


### deployDiamond

Deploy a diamond with specific facet cuts (implemented by deployment facets)

*This function is implemented by type-specific deployment facets*


```solidity
function deployDiamond(IDiamond.FacetCut[] memory facetCuts, bytes memory constructorArgs, bytes32 salt)
    external
    returns (address diamond);
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
|`diamond`|`address`|Address of the deployed diamond|


## Events
### DiamondDeployed

```solidity
event DiamondDeployed(address indexed diamond, string indexed templateName, address indexed creator);
```

### CustomDiamondDeployed

```solidity
event CustomDiamondDeployed(address indexed diamond, string[] facetNames, address indexed creator);
```

## Errors
### DeploymentFailed

```solidity
error DeploymentFailed();
```

### InvalidTemplate

```solidity
error InvalidTemplate(string templateName);
```

### InvalidFacetConfiguration

```solidity
error InvalidFacetConfiguration();
```

