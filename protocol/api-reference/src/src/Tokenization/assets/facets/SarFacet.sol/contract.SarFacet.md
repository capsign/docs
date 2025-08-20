# SarFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/facets/SarFacet.sol)

Facet providing Stock Appreciation Rights functionality

*Diamond facet for SAR creation, management, and appreciation calculation*


## Functions
### initializeSar

Initialize the SAR functionality


```solidity
function initializeSar() external;
```

### createSARToken

Create a new SAR token


```solidity
function createSARToken(address holder, uint96 quantity, uint96 basePrice) external returns (bytes32 tokenId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`holder`|`address`|The address receiving the SAR|
|`quantity`|`uint96`|The number of SAR units granted|
|`basePrice`|`uint96`|The reference price at the time of grant|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The unique identifier of this SAR token|


### createSARTokensBatch

Create multiple SAR tokens in batch


```solidity
function createSARTokensBatch(address[] memory holders, uint96[] memory quantities, uint96[] memory basePrices)
    external
    returns (bytes32[] memory tokenIds);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`holders`|`address[]`|Array of holder addresses|
|`quantities`|`uint96[]`|Array of quantities|
|`basePrices`|`uint96[]`|Array of base prices|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tokenIds`|`bytes32[]`|Array of created token IDs|


### invalidateSARToken

Invalidate a SAR token (e.g., if fully settled or holder leaves)


```solidity
function invalidateSARToken(bytes32 tokenId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID to invalidate|


### invalidateSARTokensBatch

Invalidate multiple SAR tokens in batch


```solidity
function invalidateSARTokensBatch(bytes32[] memory tokenIds) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenIds`|`bytes32[]`|Array of token IDs to invalidate|


### transferSARToken

Transfer a SAR token to a new holder


```solidity
function transferSARToken(bytes32 tokenId, address newHolder) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID to transfer|
|`newHolder`|`address`|The new holder address|


### getSARToken

Get SAR token information


```solidity
function getSARToken(bytes32 tokenId)
    external
    view
    returns (bool isValid, address holder, uint96 quantity, uint96 basePrice);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`isValid`|`bool`|Whether the token is valid|
|`holder`|`address`|The current holder|
|`quantity`|`uint96`|The number of SAR units|
|`basePrice`|`uint96`|The base price for appreciation calculation|


### isSARTokenValid

Check if a SAR token is valid


```solidity
function isSARTokenValid(bytes32 tokenId) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if valid|


### getHolderSARs

Get all SAR tokens for a holder


```solidity
function getHolderSARs(address holder) external view returns (bytes32[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`holder`|`address`|The holder address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32[]`|Array of token IDs|


### getHolderSARCount

Get the number of SAR tokens for a holder


```solidity
function getHolderSARCount(address holder) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`holder`|`address`|The holder address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The count of SAR tokens|


### getHolderTotalQuantity

Get total quantity of SARs for a holder


```solidity
function getHolderTotalQuantity(address holder) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`holder`|`address`|The holder address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total quantity|


### calculateAppreciation

Calculate appreciation for a SAR token


```solidity
function calculateAppreciation(bytes32 tokenId, uint96 currentPrice) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|
|`currentPrice`|`uint96`|The current stock price|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The appreciation amount|


### calculateHolderTotalAppreciation

Calculate total appreciation for a holder


```solidity
function calculateHolderTotalAppreciation(address holder, uint96 currentPrice) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`holder`|`address`|The holder address|
|`currentPrice`|`uint96`|The current stock price|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total appreciation amount|


### getHolderSARDetails

Get detailed SAR information for a holder


```solidity
function getHolderSARDetails(address holder)
    external
    view
    returns (
        bytes32[] memory tokenIds,
        uint96[] memory quantities,
        uint96[] memory basePrices,
        bool[] memory validFlags
    );
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`holder`|`address`|The holder address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tokenIds`|`bytes32[]`|Array of token IDs|
|`quantities`|`uint96[]`|Array of quantities|
|`basePrices`|`uint96[]`|Array of base prices|
|`validFlags`|`bool[]`|Array of validity flags|


### calculateAppreciationsBatch

Calculate appreciation for multiple SAR tokens


```solidity
function calculateAppreciationsBatch(bytes32[] memory tokenIds, uint96 currentPrice)
    external
    view
    returns (uint256[] memory appreciations);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenIds`|`bytes32[]`|Array of token IDs|
|`currentPrice`|`uint96`|The current stock price|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`appreciations`|`uint256[]`|Array of appreciation amounts|


### getHolderSARSummary

Get summary statistics for all SARs of a holder


```solidity
function getHolderSARSummary(address holder, uint96 currentPrice)
    external
    view
    returns (uint256 totalQuantity, uint256 totalAppreciation, uint256 validTokenCount, uint256 totalTokenCount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`holder`|`address`|The holder address|
|`currentPrice`|`uint96`|The current stock price|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalQuantity`|`uint256`|Total quantity of valid SARs|
|`totalAppreciation`|`uint256`|Total appreciation at current price|
|`validTokenCount`|`uint256`|Number of valid SAR tokens|
|`totalTokenCount`|`uint256`|Total number of SAR tokens (including invalid)|


### isSarInitialized

Check if SAR is initialized


```solidity
function isSarInitialized() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if initialized|


### getSARTokenWithAppreciation

Get SAR token with appreciation calculation


```solidity
function getSARTokenWithAppreciation(bytes32 tokenId, uint96 currentPrice)
    external
    view
    returns (bool isValid, address holder, uint96 quantity, uint96 basePrice, uint256 appreciation);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|
|`currentPrice`|`uint96`|The current stock price|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`isValid`|`bool`|Whether the token is valid|
|`holder`|`address`|The current holder|
|`quantity`|`uint96`|The number of SAR units|
|`basePrice`|`uint96`|The base price|
|`appreciation`|`uint256`|The current appreciation amount|


## Events
### SARTokenCreated

```solidity
event SARTokenCreated(bytes32 indexed tokenId, address indexed holder, uint96 quantity, uint96 basePrice);
```

### SARTokenInvalidated

```solidity
event SARTokenInvalidated(bytes32 indexed tokenId);
```

### SARTokenTransferred

```solidity
event SARTokenTransferred(bytes32 indexed tokenId, address indexed from, address indexed to);
```

