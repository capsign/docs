# IShareClass
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/interfaces/IShareClass.sol)

**Inherits:**
[IIssuedAsset](/src/Tokenization/assets/interfaces/IIssuedAsset.sol/interface.IIssuedAsset.md)


## Functions
### createLot

*Creates a brand-new lot (e.g., an initial issuance) for a user.*


```solidity
function createLot(
    address to,
    uint256 realAmount,
    address paymentCurrency,
    uint256 costBasis,
    uint256 acquisitionDate,
    string memory uri,
    bytes memory data,
    uint256 customId
) external returns (bytes32 tokenId, uint256 assignedCustomId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|The owner of the lot.|
|`realAmount`|`uint256`|The total *raw* quantity for this lot.|
|`paymentCurrency`|`address`|The payment currency for this lot.|
|`costBasis`|`uint256`|*Raw* cost basis per share/unit.|
|`acquisitionDate`|`uint256`|The original acquisition date.|
|`uri`|`string`|The URI of the lot.|
|`data`|`bytes`|Additional data associated with the lot.|
|`customId`|`uint256`|Optional custom ID (0 for auto-assignment).|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The unique ID of the new lot.|
|`assignedCustomId`|`uint256`|The assigned custom ID.|


### adjustLot


```solidity
function adjustLot(
    bytes32 oldLotId,
    uint256 newRealQuantity,
    uint256 newCostBasis,
    address newPaymentCurrency,
    uint256 newAcquisitionDate,
    string memory newUri,
    bytes memory newData,
    string calldata reason,
    uint256 customId
) external returns (bytes32);
```

### applyStockDividend

*Applies a stock dividend. If `reducesBasis` is true, we treat it like
a split for cost basis. Otherwise, only the share quantity ratio is updated.*


```solidity
function applyStockDividend(uint256 divNum, uint256 divDen, bool reducesBasis) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`divNum`|`uint256`|Numerator of the dividend ratio.|
|`divDen`|`uint256`|Denominator of the dividend ratio.|
|`reducesBasis`|`bool`|Whether this dividend reduces cost basis.|


### applyStockSplit

*Applies a new stock split ratio (e.g., 2-for-1). This function
updates the *cumulativeSplitNum/cumulativeSplitDen* ratio multiplicatively.*


```solidity
function applyStockSplit(uint256 splitNum, uint256 splitDen) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`splitNum`|`uint256`|Numerator of the new split ratio.|
|`splitDen`|`uint256`|Denominator of the new split ratio.|


### getRawLot

*Convenience function to return the *raw* stored data without transformations.*


```solidity
function getRawLot(bytes32 lotId)
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
        bytes memory data
    );
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID to query.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`parentLotId`|`bytes32`|Hash of the parent lot, or 0x0 if none.|
|`isValid`|`bool`|Whether the lot is active.|
|`quantity`|`uint256`|The raw quantity.|
|`paymentCurrency`|`address`|The payment currency.|
|`costBasis`|`uint256`|The raw cost basis.|
|`acquisitionDate`|`uint256`|The original acquisition timestamp.|
|`lastUpdate`|`uint256`|The last time this lot was updated.|
|`tType`|`IIssuedAsset.TransferType`|The transfer type.|
|`uri`|`string`|The URI of the lot.|
|`data`|`bytes`|Additional data associated with the lot.|


### getVotingPower

*Returns the voting power of an account.*


```solidity
function getVotingPower(address account) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address of the account.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The voting power of the account.|


## Events
### StockSplitApplied
*Events for corporate actions*


```solidity
event StockSplitApplied(uint256 splitNum, uint256 splitDen);
```

### StockDividendApplied

```solidity
event StockDividendApplied(uint256 divNum, uint256 divDen, bool reducesBasis);
```

### EntityPublicStatusUpdated
*Event that emits when the entity public status is updated.*


```solidity
event EntityPublicStatusUpdated(bool isPublic);
```

### AuthorizedAmountUpdated
*Event that emits when the authorized amount is updated.*


```solidity
event AuthorizedAmountUpdated(uint256 authorizedAmount);
```

