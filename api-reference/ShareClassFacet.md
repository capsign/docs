# ShareClassFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/facets/ShareClassFacet.sol)

Facet providing ShareClass functionality that extends IssuedAsset

*Diamond facet for corporate actions, voting power, and share management*


## Functions
### initializeShareClass

Initialize the ShareClass functionality


```solidity
function initializeShareClass() external;
```

### applyStockSplit

Apply a stock split (e.g., 2-for-1 split)


```solidity
function applyStockSplit(uint256 splitNum, uint256 splitDen) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`splitNum`|`uint256`|Numerator of the split ratio|
|`splitDen`|`uint256`|Denominator of the split ratio|


### applyStockDividend

Apply a stock dividend


```solidity
function applyStockDividend(uint256 divNum, uint256 divDen, bool reducesBasis) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`divNum`|`uint256`|Numerator of the dividend ratio (e.g., 110 for 10% dividend)|
|`divDen`|`uint256`|Denominator of the dividend ratio (e.g., 100 for 10% dividend)|
|`reducesBasis`|`bool`|Whether this dividend reduces cost basis|


### setEntityPublicStatus

Set the entity public status


```solidity
function setEntityPublicStatus(bool isPublic) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`isPublic`|`bool`|Whether the entity is SEC registered|


### setAuthorizedAmount

Set the authorized amount of shares


```solidity
function setAuthorizedAmount(uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The authorized amount|


### createLotWithRealQuantity

Create a lot with real quantity (automatically converted to raw)


```solidity
function createLotWithRealQuantity(
    address to,
    uint256 realAmount,
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
|`realAmount`|`uint256`|The real quantity (post-splits/dividends)|
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


### transferWithRealQuantity

Transfer a lot with real quantity


```solidity
function transferWithRealQuantity(
    bytes32 lotId,
    address to,
    uint256 realQuantity,
    IIssuedAsset.TransferType tType,
    uint256 newCostBasis,
    string memory uri,
    bytes memory data
) external returns (bytes32 newLotId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID to transfer from|
|`to`|`address`|The recipient|
|`realQuantity`|`uint256`|The real quantity to transfer|
|`tType`|`IIssuedAsset.TransferType`|The transfer type|
|`newCostBasis`|`uint256`|The new cost basis|
|`uri`|`string`|The URI for the new lot|
|`data`|`bytes`|Additional data|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`newLotId`|`bytes32`|The ID of the new lot created for recipient|


### getVotingPower

Get voting power for an account


```solidity
function getVotingPower(address account) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The account to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The voting power (real quantity)|


### getTotalRealSupply

Get total real supply (adjusted for splits and dividends)


```solidity
function getTotalRealSupply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total real supply|


### getLotWithRealQuantity

Get lot information with real quantities and cost basis


```solidity
function getLotWithRealQuantity(bytes32 lotId)
    external
    view
    returns (
        bytes32 parentLotId,
        bool isValid,
        uint256 realQuantity,
        address paymentCurrency,
        uint256 realCostBasis,
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
|`lotId`|`bytes32`|The lot ID to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`parentLotId`|`bytes32`|The parent lot ID|
|`isValid`|`bool`|Whether the lot is valid|
|`realQuantity`|`uint256`|The real quantity (adjusted for splits/dividends)|
|`paymentCurrency`|`address`|The payment currency|
|`realCostBasis`|`uint256`|The real cost basis (adjusted for splits/dividends)|
|`acquisitionDate`|`uint256`|The acquisition date|
|`lastUpdate`|`uint256`|The last update timestamp|
|`tType`|`IIssuedAsset.TransferType`|The transfer type|
|`uri`|`string`|The URI|
|`data`|`bytes`|Additional data|


### getRawLot

Get raw lot information (without adjustments)


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
|`lotId`|`bytes32`|The lot ID to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`parentLotId`|`bytes32`|The parent lot ID|
|`isValid`|`bool`|Whether the lot is valid|
|`quantity`|`uint256`|The raw quantity|
|`paymentCurrency`|`address`|The payment currency|
|`costBasis`|`uint256`|The raw cost basis|
|`acquisitionDate`|`uint256`|The acquisition date|
|`lastUpdate`|`uint256`|The last update timestamp|
|`tType`|`IIssuedAsset.TransferType`|The transfer type|
|`uri`|`string`|The URI|
|`data`|`bytes`|Additional data|


### getCumulativeRatios

Get all cumulative ratios for splits and dividends


```solidity
function getCumulativeRatios()
    external
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


### getEntityInfo

Get entity information


```solidity
function getEntityInfo() external view returns (bool isPublic, uint256 authorizedAmount);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`isPublic`|`bool`|Whether the entity is SEC registered|
|`authorizedAmount`|`uint256`|The authorized amount of shares|


### isShareClassInitialized

Check if ShareClass is initialized


```solidity
function isShareClassInitialized() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if initialized|


### convertRealToRawQuantity

Convert real quantity to raw quantity


```solidity
function convertRealToRawQuantity(uint256 realQuantity) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`realQuantity`|`uint256`|The real quantity|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The raw quantity|


### convertRawToRealQuantity

Convert raw quantity to real quantity


```solidity
function convertRawToRealQuantity(uint256 rawQuantity) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`rawQuantity`|`uint256`|The raw quantity|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The real quantity|


### convertRawToRealCostBasis

Convert raw cost basis to real cost basis


```solidity
function convertRawToRealCostBasis(uint256 rawBasis) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`rawBasis`|`uint256`|The raw cost basis|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The real cost basis|


### getUserRawTotal

Get user raw total for voting calculations


```solidity
function getUserRawTotal(address account) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The account to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The raw total for the account|


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


### _processTransfer

Process transfer through ComplianceFacet


```solidity
function _processTransfer(address from, address to, bytes32 lotId, uint256 quantity) internal;
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
### StockSplitApplied

```solidity
event StockSplitApplied(uint256 splitNum, uint256 splitDen);
```

### StockDividendApplied

```solidity
event StockDividendApplied(uint256 divNum, uint256 divDen, bool reducesBasis);
```

### EntityPublicStatusUpdated

```solidity
event EntityPublicStatusUpdated(bool isPublic);
```

### AuthorizedAmountUpdated

```solidity
event AuthorizedAmountUpdated(uint256 authorizedAmount);
```

