# SafeStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/storage/SafeStorage.sol)

Diamond storage library for SAFE (Simple Agreement for Future Equity) functionality

*Uses deterministic storage slots to avoid storage collisions with IssuedAsset*


## State Variables
### SAFE_STORAGE_POSITION

```solidity
bytes32 constant SAFE_STORAGE_POSITION = keccak256("capsign.storage.safe");
```


## Functions
### layout

Get the SAFE storage layout


```solidity
function layout() internal pure returns (Layout storage l);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`l`|`Layout`|The storage layout struct|


### initializeSafeStorage

Initialize the SAFE storage with default values


```solidity
function initializeSafeStorage() internal;
```

### setDefaultTermsInternal

Set default terms for all new SAFEs in this offering


```solidity
function setDefaultTermsInternal(
    uint256 valuationCap,
    uint256 discountRate,
    address targetEquityToken,
    bool proRataRight,
    bool hasMFN
) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`valuationCap`|`uint256`|The valuation cap (0 for no cap)|
|`discountRate`|`uint256`|The discount rate in basis points|
|`targetEquityToken`|`address`|The target equity token address|
|`proRataRight`|`bool`|Whether investor has pro-rata rights|
|`hasMFN`|`bool`|Whether lots have MFN protection|


### setLotTermsInternal

Set terms for a specific lot


```solidity
function setLotTermsInternal(bytes32 lotId, Terms memory terms) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID|
|`terms`|`Terms`|The terms to set|


### setLotTermsFromDefaultInternal

Set terms for a lot using default terms


```solidity
function setLotTermsFromDefaultInternal(bytes32 lotId) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID|


### markLotConvertedInternal

Mark a lot as converted


```solidity
function markLotConvertedInternal(bytes32 lotId) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID|


### isLotConvertedInternal

Check if a lot is converted


```solidity
function isLotConvertedInternal(bytes32 lotId) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if converted|


### getLotTermsInternal

Get terms for a specific lot


```solidity
function getLotTermsInternal(bytes32 lotId) internal view returns (Terms memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`Terms`|The terms for the lot|


### getDefaultTermsInternal

Get default terms


```solidity
function getDefaultTermsInternal() internal view returns (Terms memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`Terms`|The default terms|


### isMFNProtectedInternal

Check if a lot has MFN protection


```solidity
function isMFNProtectedInternal(bytes32 lotId) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if MFN protected|


### getMFNLotsInternal

Get all MFN protected lot IDs


```solidity
function getMFNLotsInternal() internal view returns (bytes32[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32[]`|Array of MFN protected lot IDs|


### checkAndUpdateMFNTermsInternal

Check if new terms are more favorable and update MFN lots


```solidity
function checkAndUpdateMFNTermsInternal(
    Terms memory newTerms,
    function(bytes32) internal view returns (bool) isLotValid
) internal returns (bytes32[] memory updatedLots);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newTerms`|`Terms`|The new terms to compare|
|`isLotValid`|`function (bytes32) internal view returns (bool)`|Function to check if lot is valid|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`updatedLots`|`bytes32[]`|Array of lot IDs that were updated|


### calculateConversionPriceInternal

Calculate conversion price based on SAFE terms


```solidity
function calculateConversionPriceInternal(Terms memory terms, uint256 pricePerShare, uint256 valuationAmount)
    internal
    pure
    returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`terms`|`Terms`|The SAFE terms|
|`pricePerShare`|`uint256`|The current price per share|
|`valuationAmount`|`uint256`|The current valuation|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The conversion price|


### calculateEquityAmountInternal

Calculate equity amount for conversion


```solidity
function calculateEquityAmountInternal(uint256 investmentAmount, uint256 conversionPrice, uint8 decimals)
    internal
    pure
    returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investmentAmount`|`uint256`|The SAFE investment amount|
|`conversionPrice`|`uint256`|The conversion price|
|`decimals`|`uint8`|The decimals of the target equity token|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The equity amount|


### isSafeInitialized

Check if SAFE is initialized


```solidity
function isSafeInitialized() internal view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if initialized|


### getMFNLotsCountInternal

Get MFN lots count


```solidity
function getMFNLotsCountInternal() internal view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The number of MFN protected lots|


### removeMFNLotInternal

Remove a lot from MFN tracking (when lot becomes invalid)


```solidity
function removeMFNLotInternal(bytes32 lotId) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID to remove|


## Structs
### Terms

```solidity
struct Terms {
    uint256 valuationCap;
    uint256 discountRate;
    address targetEquityToken;
    bool proRataRight;
    uint256 maturityDate;
    bool hasMFN;
}
```

### Layout

```solidity
struct Layout {
    Terms defaultTerms;
    mapping(bytes32 => Terms) lotTerms;
    mapping(bytes32 => bool) lotConverted;
    mapping(bytes32 => bool) mfnProtectedLots;
    bytes32[] mfnLots;
    bool safeInitialized;
}
```

