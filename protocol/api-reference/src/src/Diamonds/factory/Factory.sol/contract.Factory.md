# Factory
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Diamonds/factory/Factory.sol)

**Inherits:**
[Diamond](/src/Diamonds/Diamond.sol/contract.Diamond.md), [FactoryBase](/src/Diamonds/factory/FactoryBase.sol/abstract.FactoryBase.md)

Generic diamond-based factory for deploying various types of diamonds

*Combines diamond functionality with FactoryBase deployment capabilities*

*Deployment logic is handled by type-specific deployment facets*

*Can be used for wallets, assets, funds, governance, etc.*


## State Variables
### factoryType
The type of factory (e.g., "WalletFactory", "AssetFactory")


```solidity
string public factoryType;
```


## Functions
### constructor

Deploy a generic Factory diamond


```solidity
constructor(
    string memory _factoryType,
    address _facetRegistry,
    address _globalAccessManager,
    string memory _templateName
)
    Diamond(_createInitParams(_facetRegistry, _globalAccessManager, _templateName))
    FactoryBase(_facetRegistry, _globalAccessManager);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_factoryType`|`string`|The type of factory (e.g., "WalletFactory", "AssetFactory")|
|`_facetRegistry`|`address`|Address of the FacetRegistry|
|`_globalAccessManager`|`address`|Address of the GlobalAccessManager|
|`_templateName`|`string`|Name of the template to use for factory construction|


### _createInitParams

*Create initialization parameters for the diamond using FacetRegistry*


```solidity
function _createInitParams(address _facetRegistry, address _globalAccessManager, string memory _templateName)
    private
    view
    returns (Diamond.InitParams memory initParams);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_facetRegistry`|`address`|Address of the FacetRegistry|
|`_globalAccessManager`|`address`|Address of the GlobalAccessManager|
|`_templateName`|`string`|Name of the template to use for factory construction|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`initParams`|`Diamond.InitParams`|The initialization parameters|


### deployDiamond

Deploy a diamond with specific facet cuts

*This function should be overridden by deployment facets for type-specific logic*

*Falls back to default deployment if no facet override is available*


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


### receive


```solidity
receive() external payable;
```

## Events
### FactoryCreated

```solidity
event FactoryCreated(string indexed factoryType, address indexed factory, address indexed facetRegistry);
```

