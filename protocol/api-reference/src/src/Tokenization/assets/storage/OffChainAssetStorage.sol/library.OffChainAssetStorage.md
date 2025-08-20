# OffChainAssetStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/storage/OffChainAssetStorage.sol)

Storage library for off-chain asset functionality

*Manages storage for various types of off-chain assets held in custody*


## State Variables
### OFF_CHAIN_ASSET_STORAGE_POSITION

```solidity
bytes32 constant OFF_CHAIN_ASSET_STORAGE_POSITION = keccak256("capsign.storage.off_chain_asset");
```


## Functions
### layout


```solidity
function layout() internal pure returns (Layout storage l);
```

### requireNotInitialized


```solidity
function requireNotInitialized() internal view;
```

### requireInitialized


```solidity
function requireInitialized() internal view;
```

### setInitialized


```solidity
function setInitialized() internal;
```

### isInitialized


```solidity
function isInitialized() internal view returns (bool);
```

### setAssetInfo


```solidity
function setAssetInfo(AssetInfo memory assetInfo) internal;
```

### getAssetInfo


```solidity
function getAssetInfo() internal view returns (AssetInfo storage);
```

### updateValuation


```solidity
function updateValuation(uint256 newPrice, uint256 valuationDate, string memory source) internal;
```

### setAuthorizedCustodian


```solidity
function setAuthorizedCustodian(address custodian, bool authorized) internal;
```

### isAuthorizedCustodian


```solidity
function isAuthorizedCustodian(address custodian) internal view returns (bool);
```

### addCustodyAttestation


```solidity
function addCustodyAttestation(CustodyAttestation memory attestation) internal;
```

### getCustodyAttestation


```solidity
function getCustodyAttestation(uint256 index) internal view returns (CustodyAttestation memory);
```

### getCustodyHistoryLength


```solidity
function getCustodyHistoryLength() internal view returns (uint256);
```

### getTotalAssetsInCustody


```solidity
function getTotalAssetsInCustody() internal view returns (uint256);
```

### getLatestCustodyProof


```solidity
function getLatestCustodyProof() internal view returns (string memory);
```

### getLastCustodyUpdate


```solidity
function getLastCustodyUpdate() internal view returns (uint256);
```

### assetTypeToUint

Get asset type as uint256 for compatibility


```solidity
function assetTypeToUint(AssetType assetType) internal pure returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`assetType`|`AssetType`|The asset type enum|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The asset type as uint256|


### uintToAssetType

Convert uint256 to asset type


```solidity
function uintToAssetType(uint256 value) internal pure returns (AssetType);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`value`|`uint256`|The uint256 value|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`AssetType`|The asset type enum|


### isValidAssetType

Check if asset type is valid


```solidity
function isValidAssetType(AssetType assetType) internal pure returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`assetType`|`AssetType`|The asset type to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if valid|


### getCurrentMarketCap

Get current market cap based on custody and valuation


```solidity
function getCurrentMarketCap() internal view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Market cap in USD (scaled)|


### hasSufficientCustody

Check if there are sufficient assets in custody for tokenization


```solidity
function hasSufficientCustody(uint256 requestedQuantity) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requestedQuantity`|`uint256`|The quantity requested for tokenization|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if sufficient assets available|


## Structs
### AssetInfo

```solidity
struct AssetInfo {
    string assetName;
    AssetType assetType;
    string primaryIdentifier;
    string secondaryIdentifier;
    string description;
    string website;
    uint256 totalAvailableUnits;
    uint256 lastValuationPrice;
    uint256 lastValuationDate;
    string lastValuationSource;
}
```

### CustodyAttestation

```solidity
struct CustodyAttestation {
    uint256 quantity;
    string custodyProof;
    address attestor;
    uint256 timestamp;
    bytes signature;
}
```

### Layout

```solidity
struct Layout {
    bool initialized;
    AssetInfo assetInfo;
    mapping(address => bool) authorizedCustodians;
    CustodyAttestation[] custodyHistory;
    uint256 totalAssetsInCustody;
}
```

## Enums
### AssetType

```solidity
enum AssetType {
    UNKNOWN,
    PRIVATE_EQUITY,
    PUBLIC_STOCK,
    CRYPTOCURRENCY,
    COMMODITY,
    REAL_ESTATE,
    BOND,
    DERIVATIVE,
    OTHER
}
```

