# AssetFactoryStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/factory/storage/AssetFactoryStorage.sol)

Simplified storage layout for AssetFactory diamond using unified FacetRegistry

*Registry functionality moved to unified FacetRegistry - this only tracks deployments*


## State Variables
### ASSET_FACTORY_STORAGE_POSITION

```solidity
bytes32 constant ASSET_FACTORY_STORAGE_POSITION = keccak256("capsign.storage.asset_factory");
```


## Functions
### layout


```solidity
function layout() internal pure returns (Layout storage l);
```

### requireInitialized


```solidity
function requireInitialized() internal view;
```

### requireNotInitialized


```solidity
function requireNotInitialized() internal view;
```

### setInitialized


```solidity
function setInitialized() internal;
```

### addDeployedAsset


```solidity
function addDeployedAsset(address asset, string memory assetType) internal;
```

### getAllAssets


```solidity
function getAllAssets() internal view returns (address[] memory);
```

### getAssetsByType


```solidity
function getAssetsByType(string memory assetType) internal view returns (address[] memory);
```

### getAssetTypeByAddress


```solidity
function getAssetTypeByAddress(address asset) internal view returns (string memory);
```

### getDeploymentCount


```solidity
function getDeploymentCount(string memory assetType) internal view returns (uint256);
```

### getTransferControllerImpl


```solidity
function getTransferControllerImpl() internal view returns (address);
```

### setTransferControllerImpl


```solidity
function setTransferControllerImpl(address impl) internal;
```

### getVersion


```solidity
function getVersion() internal view returns (uint256);
```

### incrementVersion


```solidity
function incrementVersion() internal;
```

## Structs
### Layout

```solidity
struct Layout {
    mapping(address => string) assetToType;
    address[] allAssets;
    mapping(string => address[]) assetsByType;
    mapping(string => uint256) deploymentCount;
    address transferControllerImpl;
    uint256 version;
    bool initialized;
    uint256[50] __gap;
}
```

