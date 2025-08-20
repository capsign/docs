# FundDeploymentFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Funds/factory/facets/FundDeploymentFacet.sol)

Facet for deploying funds using unified FacetRegistry approach

*Uses FacetRegistry for template management instead of local fund types*


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


### deployFund

Deploy a fund of specified type using FacetRegistry template


```solidity
function deployFund(
    string memory fundTypeName,
    address entity,
    string memory name,
    string memory symbol,
    IUniversalFund.FundConfig memory fundConfig,
    bytes memory customInitData
) external returns (address fundAddress);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`fundTypeName`|`string`|Type of fund to deploy (must exist in FacetRegistry)|
|`entity`|`address`|Address of the fund entity|
|`name`|`string`|Name of the fund|
|`symbol`|`string`|Symbol of the fund|
|`fundConfig`|`IUniversalFund.FundConfig`|Configuration for the fund|
|`customInitData`|`bytes`|Custom initialization data|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`fundAddress`|`address`|Address of the deployed fund|


### _initializeFund

Initialize fund with configuration


```solidity
function _initializeFund(
    address fundAddress,
    address entity,
    string memory name,
    string memory symbol,
    IUniversalFund.FundConfig memory fundConfig,
    bytes memory customInitData
) internal;
```

### isFundTypeDeployable

Check if fund type is deployable via FacetRegistry


```solidity
function isFundTypeDeployable(string memory fundTypeName) external view returns (bool);
```

### _requireAuthorized

Check authorization


```solidity
function _requireAuthorized() internal view;
```

## Events
### FundCreated

```solidity
event FundCreated(address indexed fund, address indexed entity, string fundTypeName, string name, string symbol);
```

## Errors
### FundDeployment_TemplateNotFound

```solidity
error FundDeployment_TemplateNotFound();
```

### FundDeployment_InvalidEntity

```solidity
error FundDeployment_InvalidEntity();
```

### FundDeployment_NameRequired

```solidity
error FundDeployment_NameRequired();
```

### FundDeployment_SymbolRequired

```solidity
error FundDeployment_SymbolRequired();
```

### FundDeployment_InitializationFailed

```solidity
error FundDeployment_InitializationFailed();
```

### FundDeployment_Unauthorized

```solidity
error FundDeployment_Unauthorized();
```

