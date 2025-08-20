# AssetDeploymentFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/factory/facets/AssetDeploymentFacet.sol)

Facet for deploying assets using unified FacetRegistry approach

*Uses FacetRegistry for template management instead of local asset types*


## Functions
### deployDiamond

Deploy a diamond using FactoryBase pattern


```solidity
function deployDiamond(IDiamond.FacetCut[] memory cuts, bytes memory initData, bytes32 salt)
    external
    returns (address diamondAddress);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`cuts`|`IDiamond.FacetCut[]`|Array of facet cuts to install|
|`initData`|`bytes`|Initialization data for the diamond|
|`salt`|`bytes32`|Salt for deterministic deployment|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`diamondAddress`|`address`|The address of the deployed diamond|


### createAsset

Create a new asset diamond using FacetRegistry template


```solidity
function createAsset(
    string memory assetTypeName,
    address entity,
    string memory name,
    string memory symbol,
    string memory baseURI,
    bytes memory customInitData
) external returns (address assetAddress);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`assetTypeName`|`string`|The name of the asset type template in FacetRegistry|
|`entity`|`address`|The entity of the asset|
|`name`|`string`|The name of the asset|
|`symbol`|`string`|The symbol of the asset|
|`baseURI`|`string`|The base URI for metadata|
|`customInitData`|`bytes`|Custom initialization data for this specific asset|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`assetAddress`|`address`|The address of the created asset diamond|


### createAssetWithEASAndSanctions

Create a new asset and immediately configure EAS registry and default compliance conditions

*Convenience wrapper: deploys the diamond, initializes core, then wires AttestationFacet and ComplianceFacet*


```solidity
function createAssetWithEASAndSanctions(
    string memory assetTypeName,
    address entity,
    string memory name,
    string memory symbol,
    string memory baseURI,
    address attestationRegistry,
    bool enableSanctions
) external returns (address assetAddress);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`assetTypeName`|`string`|The name of the asset type template in FacetRegistry|
|`entity`|`address`|The entity/owner of the asset|
|`name`|`string`|The asset name|
|`symbol`|`string`|The asset symbol|
|`baseURI`|`string`|The base URI for metadata|
|`attestationRegistry`|`address`|Address of the AttestationRegistry diamond to use (EAS-backed)|
|`enableSanctions`|`bool`|Whether to enable the Sanctions condition immediately|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`assetAddress`|`address`|The address of the created and configured asset diamond|


### _initializeAsset

Initialize the asset after deployment


```solidity
function _initializeAsset(
    address assetAddress,
    address entity,
    string memory name,
    string memory symbol,
    string memory baseURI,
    bytes memory customInitData
) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`assetAddress`|`address`|The asset diamond address|
|`entity`|`address`|The entity address|
|`name`|`string`|The asset name|
|`symbol`|`string`|The asset symbol|
|`baseURI`|`string`|The base URI|
|`customInitData`|`bytes`|Custom initialization data|


### isAssetTypeDeployable

Check if an asset type is deployable via FacetRegistry


```solidity
function isAssetTypeDeployable(string memory assetTypeName) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`assetTypeName`|`string`|Name of the asset type template|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the template exists in FacetRegistry|


### _requireAuthorized

Check if the caller is authorized to create assets


```solidity
function _requireAuthorized() internal view;
```

## Events
### AssetCreated

```solidity
event AssetCreated(
    address indexed assetAddress, address indexed creator, string indexed assetType, string name, string symbol
);
```

## Errors
### AssetDeployment_TemplateNotFound

```solidity
error AssetDeployment_TemplateNotFound();
```

### AssetDeployment_InvalidEntity

```solidity
error AssetDeployment_InvalidEntity();
```

### AssetDeployment_NameRequired

```solidity
error AssetDeployment_NameRequired();
```

### AssetDeployment_SymbolRequired

```solidity
error AssetDeployment_SymbolRequired();
```

### AssetDeployment_InitializationFailed

```solidity
error AssetDeployment_InitializationFailed();
```

### AssetDeployment_Unauthorized

```solidity
error AssetDeployment_Unauthorized();
```

