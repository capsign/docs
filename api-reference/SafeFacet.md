# SafeFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/facets/SafeFacet.sol)

Facet providing SAFE (Simple Agreement for Future Equity) functionality

*Diamond facet for SAFE terms management, MFN protection, and conversion logic*


## Functions
### _isValidLot

Helper function to check if a lot is valid


```solidity
function _isValidLot(bytes32 lotId) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if lot is valid|


### initializeSafe

Initialize the SAFE functionality


```solidity
function initializeSafe() external;
```

### setDefaultTerms

Set default terms for all new SAFEs in this offering


```solidity
function setDefaultTerms(
    uint256 valuationCap,
    uint256 discountRate,
    address targetEquityToken,
    bool proRataRight,
    bool hasMFN
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`valuationCap`|`uint256`|The valuation cap (0 for no cap)|
|`discountRate`|`uint256`|The discount rate in basis points (2000 = 20%)|
|`targetEquityToken`|`address`|The target equity token address|
|`proRataRight`|`bool`|Whether investor has pro-rata rights|
|`hasMFN`|`bool`|Whether lots have MFN protection|


### createSafeLot

Create a lot with default SAFE terms


```solidity
function createSafeLot(
    address to,
    uint256 quantity,
    address paymentCurrency,
    uint256 costBasis,
    uint256 acquisitionDate,
    string memory uri,
    bytes memory data,
    uint256 customId
) external returns (bytes32 lotId, uint256 assignedCustomId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|The owner of the lot|
|`quantity`|`uint256`|The quantity of the lot|
|`paymentCurrency`|`address`|The payment currency|
|`costBasis`|`uint256`|The cost basis per unit|
|`acquisitionDate`|`uint256`|The acquisition date|
|`uri`|`string`|The URI for the lot|
|`data`|`bytes`|Additional data|
|`customId`|`uint256`|Optional custom ID (0 for auto-assignment)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The unique ID of the new lot|
|`assignedCustomId`|`uint256`|The assigned custom ID|


### createSafeLotWithTerms

Create a lot with custom SAFE terms


```solidity
function createSafeLotWithTerms(
    address to,
    uint256 quantity,
    address paymentCurrency,
    uint256 costBasis,
    uint256 acquisitionDate,
    string memory uri,
    bytes memory data,
    SafeStorage.Terms memory terms
) external returns (bytes32 lotId, uint256 assignedCustomId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|The owner of the lot|
|`quantity`|`uint256`|The quantity of the lot|
|`paymentCurrency`|`address`|The payment currency|
|`costBasis`|`uint256`|The cost basis per unit|
|`acquisitionDate`|`uint256`|The acquisition date|
|`uri`|`string`|The URI for the lot|
|`data`|`bytes`|Additional data|
|`terms`|`SafeStorage.Terms`|The SAFE terms for this lot|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The unique ID of the new lot|
|`assignedCustomId`|`uint256`|The assigned custom ID|


### convertToEquity

Convert SAFE to equity


```solidity
function convertToEquity(bytes32 lotId, uint256 pricePerShare, uint256 valuationAmount)
    external
    returns (bytes32 equityLotId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The SAFE lot ID to convert|
|`pricePerShare`|`uint256`|The current price per share|
|`valuationAmount`|`uint256`|The current valuation|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`equityLotId`|`bytes32`|The ID of the new equity lot|


### processMFNForNewTerms

Process MFN updates for new terms without creating a lot


```solidity
function processMFNForNewTerms(SafeStorage.Terms memory terms) external returns (bytes32[] memory updatedLots);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`terms`|`SafeStorage.Terms`|The new terms to compare against existing MFN lots|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`updatedLots`|`bytes32[]`|Array of lot IDs that were updated|


### getLotTerms

Get SAFE terms for a specific lot


```solidity
function getLotTerms(bytes32 lotId) external view returns (SafeStorage.Terms memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`SafeStorage.Terms`|The SAFE terms|


### getDefaultTerms

Get default SAFE terms


```solidity
function getDefaultTerms() external view returns (SafeStorage.Terms memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`SafeStorage.Terms`|The default SAFE terms|


### isLotConverted

Check if a lot is converted


```solidity
function isLotConverted(bytes32 lotId) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if converted|


### isMFNProtected

Check if a lot has MFN protection


```solidity
function isMFNProtected(bytes32 lotId) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if MFN protected|


### getMFNLots

Get all MFN protected lot IDs


```solidity
function getMFNLots() external view returns (bytes32[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32[]`|Array of MFN protected lot IDs|


### getMFNLotsCount

Get number of MFN protected lots


```solidity
function getMFNLotsCount() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The count of MFN protected lots|


### calculateConversionPrice

Calculate conversion price for a SAFE lot


```solidity
function calculateConversionPrice(bytes32 lotId, uint256 pricePerShare, uint256 valuationAmount)
    external
    view
    returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID|
|`pricePerShare`|`uint256`|The current price per share|
|`valuationAmount`|`uint256`|The current valuation|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The conversion price|


### calculateEquityAmount

Calculate equity amount for a SAFE conversion


```solidity
function calculateEquityAmount(bytes32 lotId, uint256 pricePerShare, uint256 valuationAmount)
    external
    view
    returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID|
|`pricePerShare`|`uint256`|The current price per share|
|`valuationAmount`|`uint256`|The current valuation|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The equity amount that would be received|


### isSafeInitialized

Check if SAFE is initialized


```solidity
function isSafeInitialized() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if initialized|


### getSafeLotInfo

Get lot information with conversion status


```solidity
function getSafeLotInfo(bytes32 lotId)
    external
    view
    returns (
        bytes32 parentLotId,
        bool isValid,
        uint256 quantity,
        address paymentCurrency,
        uint256 costBasis,
        uint256 acquisitionDate,
        uint256 lastUpdate,
        IIssuedAsset.TransferType tType,
        string memory uri,
        bytes memory data,
        bool isConverted,
        SafeStorage.Terms memory terms
    );
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`parentLotId`|`bytes32`|The parent lot ID|
|`isValid`|`bool`|Whether the lot is valid|
|`quantity`|`uint256`|The quantity|
|`paymentCurrency`|`address`|The payment currency|
|`costBasis`|`uint256`|The cost basis|
|`acquisitionDate`|`uint256`|The acquisition date|
|`lastUpdate`|`uint256`|The last update timestamp|
|`tType`|`IIssuedAsset.TransferType`|The transfer type|
|`uri`|`string`|The URI|
|`data`|`bytes`|Additional data|
|`isConverted`|`bool`|Whether the SAFE is converted|
|`terms`|`SafeStorage.Terms`|The SAFE terms|


### _validateCompliance

Validate compliance through ComplianceFacet


```solidity
function _validateCompliance(address from, address to, bytes32 lotId, uint256 quantity) internal view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The sender address|
|`to`|`address`|The recipient address|
|`lotId`|`bytes32`|The lot ID being transferred|
|`quantity`|`uint256`|The quantity being transferred|


### _processIssuance

Process issuance through ComplianceFacet


```solidity
function _processIssuance(address to, bytes32 lotId, uint256 quantity, bytes memory data) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|The recipient address|
|`lotId`|`bytes32`|The lot ID being issued|
|`quantity`|`uint256`|The quantity being issued|
|`data`|`bytes`|Additional issuance data|


## Events
### DefaultTermsSet

```solidity
event DefaultTermsSet(
    uint256 valuationCap, uint256 discountRate, address targetEquityToken, bool proRataRight, bool hasMFN
);
```

### LotTermsSet

```solidity
event LotTermsSet(bytes32 indexed lotId, uint256 valuationCap, uint256 discountRate);
```

### SafeConverted

```solidity
event SafeConverted(bytes32 indexed lotId, address indexed investor, bytes32 indexed equityLotId);
```

### MFNTermsUpdated

```solidity
event MFNTermsUpdated(bytes32 indexed lotId, uint256 newValuationCap, uint256 newDiscountRate);
```

