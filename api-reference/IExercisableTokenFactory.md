# IExercisableTokenFactory
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/interfaces/IExercisableToken.sol)

*Interface for creating exercisable tokens (implemented by asset facets)*


## Functions
### createExercisableToken

*Create a new exercisable token*


```solidity
function createExercisableToken(
    address holder,
    uint96 quantity,
    address paymentCurrency,
    uint96 strikePrice,
    uint64 grantDate,
    uint64 expirationDate
) external returns (bytes32 tokenId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`holder`|`address`|The token holder|
|`quantity`|`uint96`|The token quantity|
|`paymentCurrency`|`address`|The payment currency|
|`strikePrice`|`uint96`|The strike price|
|`grantDate`|`uint64`|The grant date|
|`expirationDate`|`uint64`|The expiration date|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The created token ID|


### batchCreateExercisableTokens

*Batch create exercisable tokens*


```solidity
function batchCreateExercisableTokens(
    address[] calldata holders,
    uint96[] calldata quantities,
    address paymentCurrency,
    uint96 strikePrice,
    uint64 grantDate,
    uint64 expirationDate
) external returns (bytes32[] memory tokenIds);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`holders`|`address[]`|Array of token holders|
|`quantities`|`uint96[]`|Array of token quantities|
|`paymentCurrency`|`address`|The payment currency (same for all)|
|`strikePrice`|`uint96`|The strike price (same for all)|
|`grantDate`|`uint64`|The grant date (same for all)|
|`expirationDate`|`uint64`|The expiration date (same for all)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tokenIds`|`bytes32[]`|Array of created token IDs|


## Events
### ExercisableTokenCreated

```solidity
event ExercisableTokenCreated(
    bytes32 indexed tokenId,
    address indexed holder,
    uint96 quantity,
    address paymentCurrency,
    uint96 strikePrice,
    uint64 grantDate,
    uint64 expirationDate
);
```

