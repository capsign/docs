# WarrantFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/facets/WarrantFacet.sol)

Facet providing Warrant functionality

*Diamond facet for warrant creation, management, and exercise calculations*


## Functions
### initializeWarrant

Initialize the Warrant functionality


```solidity
function initializeWarrant() external;
```

### createWarrantToken

Create a new warrant token


```solidity
function createWarrantToken(address owner, uint96 quantity, address paymentCurrency, uint96 strikePrice)
    external
    returns (bytes32 tokenId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner of the warrant|
|`quantity`|`uint96`|The number of warrants|
|`paymentCurrency`|`address`|The currency in which the warrants are denominated|
|`strikePrice`|`uint96`|The strike price for exercising the warrant|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The unique identifier of this warrant token|


### createWarrantTokensBatch

Create multiple warrant tokens in batch


```solidity
function createWarrantTokensBatch(
    address[] memory owners,
    uint96[] memory quantities,
    address[] memory paymentCurrencies,
    uint96[] memory strikePrices
) external returns (bytes32[] memory tokenIds);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owners`|`address[]`|Array of owner addresses|
|`quantities`|`uint96[]`|Array of quantities|
|`paymentCurrencies`|`address[]`|Array of payment currencies|
|`strikePrices`|`uint96[]`|Array of strike prices|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tokenIds`|`bytes32[]`|Array of created token IDs|


### invalidateWarrantToken

Invalidate a warrant token


```solidity
function invalidateWarrantToken(bytes32 tokenId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID to invalidate|


### invalidateWarrantTokensBatch

Invalidate multiple warrant tokens in batch


```solidity
function invalidateWarrantTokensBatch(bytes32[] memory tokenIds) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenIds`|`bytes32[]`|Array of token IDs to invalidate|


### transferWarrantToken

Transfer a warrant token to a new owner


```solidity
function transferWarrantToken(bytes32 tokenId, address newOwner) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID to transfer|
|`newOwner`|`address`|The new owner address|


### getWarrantToken

Get warrant token information


```solidity
function getWarrantToken(bytes32 tokenId)
    external
    view
    returns (bool isValid, address owner, uint96 quantity, address paymentCurrency, uint96 strikePrice);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`isValid`|`bool`|Whether the token is valid|
|`owner`|`address`|The current owner|
|`quantity`|`uint96`|The number of warrants|
|`paymentCurrency`|`address`|The payment currency|
|`strikePrice`|`uint96`|The strike price|


### isWarrantTokenValid

Check if a warrant token is valid


```solidity
function isWarrantTokenValid(bytes32 tokenId) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if valid|


### getOwnerWarrants

Get all warrant tokens for an owner


```solidity
function getOwnerWarrants(address owner) external view returns (bytes32[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32[]`|Array of token IDs|


### getOwnerWarrantCount

Get the number of warrant tokens for an owner


```solidity
function getOwnerWarrantCount(address owner) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The count of warrant tokens|


### getOwnerTotalQuantity

Get total quantity of warrants for an owner


```solidity
function getOwnerTotalQuantity(address owner) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total quantity|


### calculateExerciseValue

Calculate exercise value for a warrant token


```solidity
function calculateExerciseValue(bytes32 tokenId, uint96 currentPrice) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|
|`currentPrice`|`uint96`|The current stock price|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The exercise value|


### calculateExerciseCost

Calculate exercise cost for a warrant token


```solidity
function calculateExerciseCost(bytes32 tokenId) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total cost to exercise|


### calculateOwnerTotalExerciseValue

Calculate total exercise value for an owner


```solidity
function calculateOwnerTotalExerciseValue(address owner, uint96 currentPrice) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|
|`currentPrice`|`uint96`|The current stock price|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total exercise value|


### calculateOwnerTotalExerciseCost

Calculate total exercise cost for an owner


```solidity
function calculateOwnerTotalExerciseCost(address owner) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total exercise cost|


### isInTheMoney

Check if warrant is in the money


```solidity
function isInTheMoney(bytes32 tokenId, uint96 currentPrice) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|
|`currentPrice`|`uint96`|The current stock price|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if current price > strike price|


### getOwnerWarrantDetails

Get detailed warrant information for an owner


```solidity
function getOwnerWarrantDetails(address owner)
    external
    view
    returns (
        bytes32[] memory tokenIds,
        uint96[] memory quantities,
        address[] memory paymentCurrencies,
        uint96[] memory strikePrices,
        bool[] memory validFlags
    );
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tokenIds`|`bytes32[]`|Array of token IDs|
|`quantities`|`uint96[]`|Array of quantities|
|`paymentCurrencies`|`address[]`|Array of payment currencies|
|`strikePrices`|`uint96[]`|Array of strike prices|
|`validFlags`|`bool[]`|Array of validity flags|


### calculateExerciseValuesBatch

Calculate exercise values for multiple warrant tokens


```solidity
function calculateExerciseValuesBatch(bytes32[] memory tokenIds, uint96 currentPrice)
    external
    view
    returns (uint256[] memory exerciseValues);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenIds`|`bytes32[]`|Array of token IDs|
|`currentPrice`|`uint96`|The current stock price|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`exerciseValues`|`uint256[]`|Array of exercise values|


### getOwnerWarrantSummary

Get summary statistics for all warrants of an owner


```solidity
function getOwnerWarrantSummary(address owner, uint96 currentPrice)
    external
    view
    returns (
        uint256 totalQuantity,
        uint256 totalExerciseValue,
        uint256 totalExerciseCost,
        uint256 inTheMoneyCount,
        uint256 validTokenCount,
        uint256 totalTokenCount
    );
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|
|`currentPrice`|`uint96`|The current stock price|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalQuantity`|`uint256`|Total quantity of valid warrants|
|`totalExerciseValue`|`uint256`|Total exercise value at current price|
|`totalExerciseCost`|`uint256`|Total cost to exercise all warrants|
|`inTheMoneyCount`|`uint256`|Number of warrants that are in the money|
|`validTokenCount`|`uint256`|Number of valid warrant tokens|
|`totalTokenCount`|`uint256`|Total number of warrant tokens (including invalid)|


### isWarrantInitialized

Check if Warrant is initialized


```solidity
function isWarrantInitialized() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if initialized|


### getWarrantTokenWithExercise

Get warrant token with exercise calculations


```solidity
function getWarrantTokenWithExercise(bytes32 tokenId, uint96 currentPrice)
    external
    view
    returns (
        bool isValid,
        address owner,
        uint96 quantity,
        address paymentCurrency,
        uint96 strikePrice,
        uint256 exerciseValue,
        uint256 exerciseCost,
        bool inTheMoney
    );
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
|`owner`|`address`|The current owner|
|`quantity`|`uint96`|The number of warrants|
|`paymentCurrency`|`address`|The payment currency|
|`strikePrice`|`uint96`|The strike price|
|`exerciseValue`|`uint256`|The current exercise value|
|`exerciseCost`|`uint256`|The cost to exercise|
|`inTheMoney`|`bool`|Whether the warrant is in the money|


## Events
### WarrantTokenCreated

```solidity
event WarrantTokenCreated(
    bytes32 indexed tokenId, address indexed owner, uint96 quantity, address paymentCurrency, uint96 strikePrice
);
```

### WarrantTokenInvalidated

```solidity
event WarrantTokenInvalidated(bytes32 indexed tokenId);
```

### WarrantTokenTransferred

```solidity
event WarrantTokenTransferred(bytes32 indexed tokenId, address indexed from, address indexed to);
```

