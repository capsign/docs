# FacetRegistry
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/registry/FacetRegistry.sol)

**Inherits:**
[IFacetRegistry](/src/Diamonds/registry/IFacetRegistry.sol/interface.IFacetRegistry.md), [FacetRegistryBase](/src/Diamonds/registry/FacetRegistryBase.sol/abstract.FacetRegistryBase.md), [AccessManaged](/src/Authorization/AccessManaged.sol/abstract.AccessManaged.md)

Facet registry with name-based lookups, templates, and dynamic composition

*Extends the base FacetRegistry with advanced features for factory usage*


## State Variables
### facetsByName

```solidity
mapping(string => IFacetRegistry.FacetInfo) public facetsByName;
```


### templates

```solidity
mapping(string => IFacetRegistry.FacetTemplate) public templates;
```


### facetsByCategory

```solidity
mapping(string => string[]) public facetsByCategory;
```


### allFacetNames

```solidity
string[] public allFacetNames;
```


### allTemplateNames

```solidity
string[] public allTemplateNames;
```


### allCategories

```solidity
string[] public allCategories;
```


## Functions
### constructor


```solidity
constructor(address _globalAccessManager) AccessManaged(_globalAccessManager);
```

### registerFacetByName

Register a facet with name-based lookup


```solidity
function registerFacetByName(
    string memory name,
    address facetAddress,
    bytes4[] memory selectors,
    string memory description,
    string memory category
) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|Unique name for the facet|
|`facetAddress`|`address`|Address of the deployed facet|
|`selectors`|`bytes4[]`|Function selectors the facet implements|
|`description`|`string`|Human-readable description|
|`category`|`string`|Category for organization (e.g., "core", "governance")|


### updateFacet

Update a facet's address (for upgrades)


```solidity
function updateFacet(string memory name, address newFacetAddress) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|Name of the facet to update|
|`newFacetAddress`|`address`|New address of the facet|


### updateFacetWithSelectors

Update a facet's address and selectors (for major upgrades)


```solidity
function updateFacetWithSelectors(string memory name, address newFacetAddress, bytes4[] memory newSelectors)
    external
    restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|Name of the facet to update|
|`newFacetAddress`|`address`|New address of the facet|
|`newSelectors`|`bytes4[]`|New function selectors of the facet|


### deactivateFacet

Deactivate a facet (soft delete)


```solidity
function deactivateFacet(string memory name) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|Name of the facet to deactivate|


### registerTemplate

Register a template for facet combinations


```solidity
function registerTemplate(
    string memory name,
    string memory description,
    string[] memory requiredFacets,
    string[] memory optionalFacets,
    bytes memory initializationData
) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|Unique name for the template|
|`description`|`string`|Human-readable description|
|`requiredFacets`|`string[]`|Array of required facet names|
|`optionalFacets`|`string[]`|Array of optional facet names|
|`initializationData`|`bytes`|Default initialization data|


### updateTemplate

Update a template


```solidity
function updateTemplate(
    string memory name,
    string memory description,
    string[] memory requiredFacets,
    string[] memory optionalFacets,
    bytes memory initializationData
) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|Name of the template to update|
|`description`|`string`|New description|
|`requiredFacets`|`string[]`|New required facets|
|`optionalFacets`|`string[]`|New optional facets|
|`initializationData`|`bytes`|New initialization data|


### deactivateTemplate

Deactivate a template


```solidity
function deactivateTemplate(string memory name) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|Name of the template to deactivate|


### buildFacetCutsFromTemplate

Build facet cuts from a template


```solidity
function buildFacetCutsFromTemplate(string memory templateName)
    external
    view
    returns (IDiamond.FacetCut[] memory cuts);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`templateName`|`string`|Name of the template|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`cuts`|`IDiamond.FacetCut[]`|Array of facet cuts ready for diamond deployment|


### buildFacetCutsFromNames

Build facet cuts from custom facet list


```solidity
function buildFacetCutsFromNames(string[] memory facetNames) external view returns (IDiamond.FacetCut[] memory cuts);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`facetNames`|`string[]`|Array of facet names|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`cuts`|`IDiamond.FacetCut[]`|Array of facet cuts ready for diamond deployment|


### buildFacetCutsFromConfig

Build facet cuts from deployment configuration


```solidity
function buildFacetCutsFromConfig(IFacetRegistry.DeploymentConfig memory config)
    external
    view
    returns (IDiamond.FacetCut[] memory cuts);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`config`|`IFacetRegistry.DeploymentConfig`|Deployment configuration with template and customizations|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`cuts`|`IDiamond.FacetCut[]`|Array of facet cuts ready for diamond deployment|


### getFacetByName

Get facet information by name


```solidity
function getFacetByName(string memory name) external view returns (IFacetRegistry.FacetInfo memory info);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|Name of the facet|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`info`|`IFacetRegistry.FacetInfo`|Complete facet information|


### getFacetsByCategory

Get all facets in a category


```solidity
function getFacetsByCategory(string memory category) external view returns (string[] memory facetNames);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`category`|`string`|Category name|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`facetNames`|`string[]`|Array of facet names in the category|


### getTemplate

Get template information


```solidity
function getTemplate(string memory name) external view returns (IFacetRegistry.FacetTemplate memory template);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|Name of the template|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`template`|`IFacetRegistry.FacetTemplate`|Complete template information|


### getAllFacetNames

Get all registered facet names


```solidity
function getAllFacetNames() external view returns (string[] memory names);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`names`|`string[]`|Array of all facet names|


### getAllTemplateNames

Get all registered template names


```solidity
function getAllTemplateNames() external view returns (string[] memory names);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`names`|`string[]`|Array of all template names|


### getAllCategories

Get all categories


```solidity
function getAllCategories() external view returns (string[] memory categories);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`categories`|`string[]`|Array of all category names|


### _validateFacetList


```solidity
function _validateFacetList(string[] memory facetNames) internal view;
```

### _buildFacetCuts


```solidity
function _buildFacetCuts(string[] memory facetNames) internal view returns (IDiamond.FacetCut[] memory cuts);
```

### _combineAndFilterFacets


```solidity
function _combineAndFilterFacets(
    string[] memory templateFacets,
    string[] memory customFacets,
    string[] memory excludedFacets
) internal pure returns (string[] memory result);
```

### addFacet

Registers a new facet.


```solidity
function addFacet(address facet, bytes4[] memory selectors) external;
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
function deployFacet(bytes32 salt, bytes memory creationCode, bytes4[] memory selectors)
    external
    override
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
function computeFacetAddress(bytes32 salt, bytes memory creationCode) external view override returns (address facet);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`salt`|`bytes32`|Salt used to create the address of the new facet.|
|`creationCode`|`bytes`|Creation code of the new facet.|


### facetSelectors

Returns the selectors of a registered facet.


```solidity
function facetSelectors(address facet) external view returns (bytes4[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`facet`|`address`|The address of the facet.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes4[]`|selectors The selectors of the facet.|


### facetAddresses

Returns the addresses of all registered facets.


```solidity
function facetAddresses() external view returns (address[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|facets The addresses of all registered facets.|


### upsertFacet

Update facet if exists, create if doesn't exist (upsert pattern)


```solidity
function upsertFacet(
    string memory name,
    address facetAddress,
    bytes4[] memory selectors,
    string memory description,
    string memory category
) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|Name of the facet|
|`facetAddress`|`address`|Address of the facet|
|`selectors`|`bytes4[]`|Function selectors of the facet|
|`description`|`string`|Human-readable description|
|`category`|`string`|Category for organization|


## Events
### FacetRegisteredByName

```solidity
event FacetRegisteredByName(string indexed name, address indexed facetAddress, string category);
```

### FacetUpdated

```solidity
event FacetUpdated(string indexed name, address indexed oldAddress, address indexed newAddress);
```

### FacetDeactivated

```solidity
event FacetDeactivated(string indexed name);
```

### TemplateRegistered

```solidity
event TemplateRegistered(string indexed name, string[] requiredFacets, string[] optionalFacets);
```

### TemplateUpdated

```solidity
event TemplateUpdated(string indexed name, uint256 version);
```

### TemplateDeactivated

```solidity
event TemplateDeactivated(string indexed name);
```

### CategoryAdded

```solidity
event CategoryAdded(string indexed category);
```

## Errors
### FacetNameAlreadyExists

```solidity
error FacetNameAlreadyExists(string name);
```

### FacetNameNotFound

```solidity
error FacetNameNotFound(string name);
```

### TemplateNameAlreadyExists

```solidity
error TemplateNameAlreadyExists(string name);
```

### TemplateNameNotFound

```solidity
error TemplateNameNotFound(string name);
```

### FacetNotActive

```solidity
error FacetNotActive(string name);
```

### TemplateNotActive

```solidity
error TemplateNotActive(string name);
```

### InvalidFacetList

```solidity
error InvalidFacetList();
```

### CategoryNotFound

```solidity
error CategoryNotFound(string category);
```

