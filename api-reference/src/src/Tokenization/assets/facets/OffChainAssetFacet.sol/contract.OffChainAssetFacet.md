# OffChainAssetFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/facets/OffChainAssetFacet.sol)

Facet providing off-chain asset representation for various asset types

*Creates tokenized representation of assets held in custody (stocks, crypto, private equity, etc.)*


## Functions
### onlyAuthorizedCustodian


```solidity
modifier onlyAuthorizedCustodian();
```

### initializeOffChainAsset

Initialize the off-chain asset functionality


```solidity
function initializeOffChainAsset(OffChainAssetStorage.AssetInfo memory assetInfo) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`assetInfo`|`OffChainAssetStorage.AssetInfo`|Asset information including type and identifiers|


### addCustodyAttestation

Add custody attestation from authorized custodian


```solidity
function addCustodyAttestation(uint256 quantity, string memory custodyProof, bytes memory signature)
    external
    onlyAuthorizedCustodian;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`quantity`|`uint256`|Number of units held in custody|
|`custodyProof`|`string`|IPFS hash of custody documentation|
|`signature`|`bytes`|Cryptographic signature from custodian|


### updateValuation

Update valuation information


```solidity
function updateValuation(uint256 newPrice, uint256 valuationDate, string memory source) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newPrice`|`uint256`|New price per unit|
|`valuationDate`|`uint256`|Date of valuation|
|`source`|`string`|Source of valuation (e.g., "Bloomberg", "CoinGecko", "Internal")|


### updateAssetInfo

Update asset information


```solidity
function updateAssetInfo(string memory field, string memory newValue) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`field`|`string`|The field to update|
|`newValue`|`string`|The new value|


### authorizeCustodian

Authorize a custodian to provide attestations


```solidity
function authorizeCustodian(address custodian) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`custodian`|`address`|Address of the custodian|


### revokeCustodian

Revoke custodian authorization


```solidity
function revokeCustodian(address custodian) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`custodian`|`address`|Address of the custodian|


### getMarketValue

Get current market value of tokenized assets for an address


```solidity
function getMarketValue(address holder) external view returns (uint256 totalValue);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`holder`|`address`|Address of the token holder|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalValue`|`uint256`|Total market value in USD (scaled by last valuation price)|


### getCustodyStatus

Get detailed custody information


```solidity
function getCustodyStatus()
    external
    view
    returns (uint256 assetsInCustody, string memory custodyProof, uint256 lastUpdate, uint256 attestationCount);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`assetsInCustody`|`uint256`|Current custody status and latest attestation|
|`custodyProof`|`string`|IPFS hash of latest custody proof|
|`lastUpdate`|`uint256`|Timestamp of last custody update|
|`attestationCount`|`uint256`|Number of attestations recorded|


### getCustodyAttestation

Get custody attestation by index


```solidity
function getCustodyAttestation(uint256 index) external view returns (OffChainAssetStorage.CustodyAttestation memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`index`|`uint256`|Index in custody history|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`OffChainAssetStorage.CustodyAttestation`|Custody attestation details|


### getAssetLots

Get all lots owned by an address with asset details


```solidity
function getAssetLots(address owner)
    external
    view
    returns (
        bytes32[] memory lotIds,
        uint256[] memory quantities,
        uint256[] memory costBases,
        uint256[] memory purchaseDates
    );
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|Address to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`lotIds`|`bytes32[]`|Array of lot IDs|
|`quantities`|`uint256[]`|Array of quantities for each lot|
|`costBases`|`uint256[]`|Array of cost bases for each lot|
|`purchaseDates`|`uint256[]`|Array of purchase dates for each lot|


### isAuthorizedCustodian

Check if an address is an authorized custodian


```solidity
function isAuthorizedCustodian(address custodian) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`custodian`|`address`|Address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if authorized|


### getAssetInfo

Get asset information for this off-chain asset token


```solidity
function getAssetInfo() external view returns (OffChainAssetStorage.AssetInfo memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`OffChainAssetStorage.AssetInfo`|Asset details including type, identifiers, and metadata|


### getAssetTypeString

Get asset type as string for display


```solidity
function getAssetTypeString(OffChainAssetStorage.AssetType assetType) external pure returns (string memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`assetType`|`OffChainAssetStorage.AssetType`|The asset type enum|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|String representation of asset type|


### generateSymbol

Generate token symbol from asset name and type


```solidity
function generateSymbol(string memory assetName, OffChainAssetStorage.AssetType assetType)
    external
    pure
    returns (string memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`assetName`|`string`|Name of the asset|
|`assetType`|`OffChainAssetStorage.AssetType`|Type of the asset|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|Generated symbol|


### isOffChainAssetInitialized

Check if the off-chain asset is initialized


```solidity
function isOffChainAssetInitialized() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if initialized|


## Events
### CustodyAttestationAdded

```solidity
event CustodyAttestationAdded(uint256 quantity, string custodyProof, address attestor);
```

### CustodianAuthorized

```solidity
event CustodianAuthorized(address indexed custodian);
```

### CustodianRevoked

```solidity
event CustodianRevoked(address indexed custodian);
```

### ValuationUpdated

```solidity
event ValuationUpdated(uint256 newPrice, uint256 date, string source);
```

### AssetsTokenized

```solidity
event AssetsTokenized(address indexed recipient, uint256 quantity, bytes32 lotId);
```

### OffChainAssetInitialized

```solidity
event OffChainAssetInitialized(string assetName, OffChainAssetStorage.AssetType assetType, string identifier);
```

### AssetInfoUpdated

```solidity
event AssetInfoUpdated(string field, string oldValue, string newValue);
```

## Errors
### OffChainAsset_NotAuthorizedCustodian

```solidity
error OffChainAsset_NotAuthorizedCustodian();
```

### OffChainAsset_InvalidInput

```solidity
error OffChainAsset_InvalidInput();
```

### OffChainAsset_ExceedsAvailableAssets

```solidity
error OffChainAsset_ExceedsAvailableAssets();
```

### OffChainAsset_ExceedsCustodyBalance

```solidity
error OffChainAsset_ExceedsCustodyBalance();
```

### OffChainAsset_FutureDateNotAllowed

```solidity
error OffChainAsset_FutureDateNotAllowed();
```

### OffChainAsset_IndexOutOfBounds

```solidity
error OffChainAsset_IndexOutOfBounds();
```

### OffChainAsset_InvalidAssetType

```solidity
error OffChainAsset_InvalidAssetType();
```

