# ISAFE
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/interfaces/ISAFE.sol)

**Inherits:**
[IIssuedAsset](/src/Tokenization/assets/interfaces/IIssuedAsset.sol/interface.IIssuedAsset.md)

*Interface for the SAFE (Simple Agreement for Future Equity) contract*


## Functions
### defaultTerms

Returns the default terms for new SAFEs in this offering


```solidity
function defaultTerms() external view returns (Terms memory);
```

### lotTerms

Returns the terms for a specific lot


```solidity
function lotTerms(bytes32 lotId) external view returns (Terms memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|ID of the lot|


### lotConverted

Checks if a lot has been converted


```solidity
function lotConverted(bytes32 lotId) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|ID of the lot|


### mfnProtectedLots

Checks if a lot has MFN protection


```solidity
function mfnProtectedLots(bytes32 lotId) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|ID of the lot|


### setDefaultTerms

Sets default terms for all new SAFEs in this offering


```solidity
function setDefaultTerms(
    uint256 _valuationCap,
    uint256 _discountRate,
    address _targetEquityToken,
    bool _proRataRight,
    bool _hasMFN
) external;
```

### createLotWithTerms

Creates a lot with custom terms


```solidity
function createLotWithTerms(
    address to,
    uint256 quantity,
    address paymentCurrency,
    uint256 costBasis,
    uint256 acquisitionDate,
    string memory uri,
    bytes memory data,
    Terms memory terms
) external returns (bytes32 lotId);
```

### processMFNForNewTerms

Process MFN updates for new terms


```solidity
function processMFNForNewTerms(Terms memory newTerms) external;
```

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
|`lotId`|`bytes32`|ID of the SAFE lot to convert|
|`pricePerShare`|`uint256`|Price per token at conversion|
|`valuationAmount`|`uint256`|Valuation amount for the conversion|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`equityLotId`|`bytes32`|ID of the new equity share lot created|


## Events
### TermsSet
Emitted when terms are set for a lot


```solidity
event TermsSet(bytes32 indexed lotId, uint256 valuationCap, uint256 discountRate);
```

### Converted
Emitted when a SAFE is converted to equity


```solidity
event Converted(bytes32 indexed lotId, address indexed investor, bytes32 indexed equityLotId);
```

### MFNTermsUpdated
Emitted when MFN terms are updated


```solidity
event MFNTermsUpdated(bytes32 indexed lotId, uint256 newValuationCap, uint256 newDiscountRate);
```

## Structs
### Terms
*Structure defining the terms of a SAFE agreement*


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

