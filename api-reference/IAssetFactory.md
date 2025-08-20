# IAssetFactory
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/factory/interfaces/IAssetFactory.sol)

**Inherits:**
[IFactory](/src/Diamonds/factory/IFactory.sol/interface.IFactory.md)

Interface for AssetFactory with unified access control and factory base functionality

*Combines FactoryBase functionality with asset-specific functions and unified access control*


## Functions
### createAsset


```solidity
function createAsset(
    string memory assetTypeName,
    address entity,
    string memory name,
    string memory symbol,
    string memory baseURI,
    bytes memory customInitData
) external returns (address);
```

### isAssetTypeDeployable


```solidity
function isAssetTypeDeployable(string memory assetTypeName) external view returns (bool);
```

