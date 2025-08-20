# ShareClassStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/storage/ShareClassStorage.sol)

Diamond storage library for ShareClass functionality that extends IssuedAsset

*Uses deterministic storage slots to avoid storage collisions with IssuedAsset*


## State Variables
### SHARE_CLASS_STORAGE_POSITION

```solidity
bytes32 constant SHARE_CLASS_STORAGE_POSITION = keccak256("capsign.storage.share_class");
```


## Functions
### layout

Get the ShareClass storage layout


```solidity
function layout() internal pure returns (Layout storage l);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`l`|`Layout`|The storage layout struct|


### initializeShareClassStorage

Initialize the ShareClass storage with default values


```solidity
function initializeShareClassStorage() internal;
```

### applyStockSplitInternal

Apply a stock split by updating cumulative ratios


```solidity
function applyStockSplitInternal(uint256 splitNum, uint256 splitDen) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`splitNum`|`uint256`|Numerator of the split ratio (e.g., 2 for 2-for-1)|
|`splitDen`|`uint256`|Denominator of the split ratio (e.g., 1 for 2-for-1)|


### applyStockDividendInternal

Apply a stock dividend by updating cumulative ratios


```solidity
function applyStockDividendInternal(uint256 divNum, uint256 divDen, bool reducesBasis) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`divNum`|`uint256`|Numerator of the dividend ratio (e.g., 110 for 10% dividend)|
|`divDen`|`uint256`|Denominator of the dividend ratio (e.g., 100 for 10% dividend)|
|`reducesBasis`|`bool`|Whether this dividend also reduces cost basis|


### getRealQuantityInternal

Convert raw quantity to real quantity (after splits and dividends)


```solidity
function getRealQuantityInternal(uint256 rawQuantity) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`rawQuantity`|`uint256`|The raw quantity stored in lots|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The real quantity after applying splits and dividends|


### convertRealToRawQuantityInternal

Convert real quantity to raw quantity (for storage)


```solidity
function convertRealToRawQuantityInternal(uint256 realQuantity) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`realQuantity`|`uint256`|The real quantity to convert|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The raw quantity for storage|


### getRealCostBasisInternal

Convert raw cost basis to real cost basis (after splits and dividends)


```solidity
function getRealCostBasisInternal(uint256 rawBasis) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`rawBasis`|`uint256`|The raw cost basis stored in lots|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The real cost basis after applying splits and dividend adjustments|


### getVotingPowerInternal

Get voting power for an account based on their raw totals


```solidity
function getVotingPowerInternal(address account) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The account to calculate voting power for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The voting power (real quantity)|


### getTotalRealSupplyInternal

Get total real supply (adjusted for splits and dividends)


```solidity
function getTotalRealSupplyInternal(uint256 totalSupply) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`totalSupply`|`uint256`|The raw total supply from IssuedAsset|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The real total supply|


### updateUserRawTotalsInternal

Update user raw totals when lots are created/transferred


```solidity
function updateUserRawTotalsInternal(address user, int256 rawQuantityDelta) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The user address|
|`rawQuantityDelta`|`int256`|The change in raw quantity (can be negative)|


### setEntityPublicStatusInternal

Set the entity public status


```solidity
function setEntityPublicStatusInternal(bool isPublic) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`isPublic`|`bool`|Whether the entity is SEC registered|


### setAuthorizedAmountInternal

Set the authorized amount of shares


```solidity
function setAuthorizedAmountInternal(uint256 amount) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The authorized amount|


### checkAuthorizedAmountInternal

Check if issuance would exceed authorized amount


```solidity
function checkAuthorizedAmountInternal(uint256 totalSupply, uint256 additionalRealAmount)
    internal
    view
    returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`totalSupply`|`uint256`|Current raw total supply|
|`additionalRealAmount`|`uint256`|Additional real amount to issue|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if issuance is within authorized limits|


### getCumulativeDivNumInternal

Get the cumulative dividend numerator with zero protection


```solidity
function getCumulativeDivNumInternal() internal view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The cumulative dividend numerator|


### getCumulativeRatiosInternal

Get all cumulative ratios


```solidity
function getCumulativeRatiosInternal()
    internal
    view
    returns (
        uint256 splitNum,
        uint256 splitDen,
        uint256 divNum,
        uint256 divDen,
        uint256 divCostBasisNum,
        uint256 divCostBasisDen
    );
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`splitNum`|`uint256`|Split numerator|
|`splitDen`|`uint256`|Split denominator|
|`divNum`|`uint256`|Dividend numerator|
|`divDen`|`uint256`|Dividend denominator|
|`divCostBasisNum`|`uint256`|Dividend cost basis numerator|
|`divCostBasisDen`|`uint256`|Dividend cost basis denominator|


### getEntityInfoInternal

Get entity status and authorized amount


```solidity
function getEntityInfoInternal() internal view returns (bool isPublic, uint256 authorized);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`isPublic`|`bool`|Whether entity is public|
|`authorized`|`uint256`|The authorized amount|


### isShareClassInitialized

Check if ShareClass is initialized


```solidity
function isShareClassInitialized() internal view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if initialized|


## Structs
### Layout

```solidity
struct Layout {
    uint256 cumulativeSplitNum;
    uint256 cumulativeSplitDen;
    uint256 cumulativeDivNum;
    uint256 cumulativeDivDen;
    uint256 cumulativeDivCostBasisNum;
    uint256 cumulativeDivCostBasisDen;
    mapping(address => uint256) userRawTotals;
    bool isPublicEntity;
    uint256 authorizedAmount;
    bool shareClassInitialized;
}
```

