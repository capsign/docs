# IExercisableToken
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/interfaces/IExercisableToken.sol)

Implemented by EsoFacet, WarrantFacet, and SarFacet to provide exercise functionality

*Universal interface for compensation tokens that can be exercised into shares*


## Functions
### getExercisableTokenInfo

*Get exercisable token details*


```solidity
function getExercisableTokenInfo(bytes32 tokenId) external view returns (ExercisableTokenInfo memory info);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`info`|`ExercisableTokenInfo`|The token information struct|


### getExercisableTokenDetails

*Get exercisable token details in tuple format (for backward compatibility)*


```solidity
function getExercisableTokenDetails(bytes32 tokenId)
    external
    view
    returns (bool isValid, address holder, uint96 quantity, address paymentCurrency, uint96 strikePrice);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`isValid`|`bool`|Whether the token is valid|
|`holder`|`address`|The token holder|
|`quantity`|`uint96`|The token quantity|
|`paymentCurrency`|`address`|The payment currency|
|`strikePrice`|`uint96`|The strike price per token|


### isTokenExercisable

*Check if a token is exercisable (valid and vested)*


```solidity
function isTokenExercisable(bytes32 tokenId) external view returns (bool exercisable, string memory reason);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`exercisable`|`bool`|Whether the token can be exercised|
|`reason`|`string`|Reason if not exercisable (empty if exercisable)|


### getExercisableTokens

*Get all exercisable tokens for a holder*


```solidity
function getExercisableTokens(address holder) external view returns (bytes32[] memory tokenIds);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`holder`|`address`|The token holder address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tokenIds`|`bytes32[]`|Array of exercisable token IDs|


### markTokenExercised

*Mark a token as exercised (called by ExerciseFacet)*


```solidity
function markTokenExercised(bytes32 tokenId, uint96 exercisedQuantity) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|
|`exercisedQuantity`|`uint96`|The quantity that was exercised|


### getTotalExercisableValue

*Get total exercisable value for a holder*


```solidity
function getTotalExercisableValue(address holder) external view returns (uint256 totalValue, uint256 tokenCount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`holder`|`address`|The token holder address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalValue`|`uint256`|Total value of all exercisable tokens|
|`tokenCount`|`uint256`|Number of exercisable tokens|


## Events
### TokenMarkedExercised

```solidity
event TokenMarkedExercised(bytes32 indexed tokenId, uint96 exercisedQuantity, address indexed exerciser);
```

## Structs
### ExercisableTokenInfo
*Token information structure*


```solidity
struct ExercisableTokenInfo {
    bytes32 tokenId;
    address holder;
    uint96 quantity;
    address paymentCurrency;
    uint96 strikePrice;
    uint64 grantDate;
    uint64 expirationDate;
    bool isValid;
    bool isVested;
}
```

