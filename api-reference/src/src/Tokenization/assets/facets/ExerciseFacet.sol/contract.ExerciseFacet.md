# ExerciseFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/facets/ExerciseFacet.sol)

Works with any facet implementing IExercisableToken (EsoFacet, WarrantFacet, SarFacet)

*Universal exercise facet for converting compensation instruments into shares*


## State Variables
### EXERCISE_STORAGE_SLOT

```solidity
bytes32 private constant EXERCISE_STORAGE_SLOT = keccak256("capsign.storage.ExerciseFacet");
```


## Functions
### constructor

*Constructor*


```solidity
constructor();
```

### initializeExercise

*Initialize the exercise facet with required contracts*


```solidity
function initializeExercise(address _attestationRegistry, address _targetShareClass) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_attestationRegistry`|`address`|Address of the AttestationRegistry|
|`_targetShareClass`|`address`|Address of the ShareClass to create shares in|


### exerciseToken

*Exercise compensation tokens (options, warrants, SARs) into shares*


```solidity
function exerciseToken(bytes32 tokenId, uint96 exerciseQuantity) external payable;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The ID of the compensation token|
|`exerciseQuantity`|`uint96`|The quantity to exercise|


### batchExerciseTokens

*Batch exercise multiple tokens*


```solidity
function batchExerciseTokens(bytes32[] calldata tokenIds, uint96[] calldata exerciseQuantities) external payable;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenIds`|`bytes32[]`|Array of token IDs to exercise|
|`exerciseQuantities`|`uint96[]`|Array of quantities to exercise for each token|


### checkExerciseEligibility

*Check if a token can be exercised and get cost information*


```solidity
function checkExerciseEligibility(bytes32 tokenId, uint96 quantity)
    external
    view
    returns (bool canExercise, uint96 remainingQuantity, uint256 totalCost, string memory reason);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID to check|
|`quantity`|`uint96`|The quantity to exercise|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`canExercise`|`bool`|Whether the token can be exercised|
|`remainingQuantity`|`uint96`|How much can still be exercised|
|`totalCost`|`uint256`|The cost to exercise the specified quantity|
|`reason`|`string`|Reason if cannot exercise|


### getExerciseStatus

*Get exercise status for a token*


```solidity
function getExerciseStatus(bytes32 tokenId)
    external
    view
    returns (uint96 exercisedQuantity, uint96 remainingQuantity, bool isFullyExercised);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`exercisedQuantity`|`uint96`|How much has been exercised|
|`remainingQuantity`|`uint96`|How much remains|
|`isFullyExercised`|`bool`|Whether the token is fully exercised|


### getHolderExercisableTokens

*Get all exercisable tokens for a holder with their exercise status*


```solidity
function getHolderExercisableTokens(address holder)
    external
    view
    returns (bytes32[] memory tokenIds, uint96[] memory exercisableQuantities, uint256[] memory totalValues);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`holder`|`address`|The token holder address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tokenIds`|`bytes32[]`|Array of token IDs|
|`exercisableQuantities`|`uint96[]`|Array of quantities that can be exercised|
|`totalValues`|`uint256[]`|Array of total values for each token|


### _getFairValue

*Get the current fair market value for a company*


```solidity
function _getFairValue(address company) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`company`|`address`|The company address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The latest fair market value|


### _emitValuationUsed

*Emit valuation usage event*


```solidity
function _emitValuationUsed(address company, uint256 currentFairValue) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`company`|`address`|The company address|
|`currentFairValue`|`uint256`|The current fair value|


### _getExerciseStorage

*Get exercise storage*


```solidity
function _getExerciseStorage() internal pure returns (ExerciseStorage storage s);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`s`|`ExerciseStorage`|The exercise storage struct|


### emergencyWithdraw

*Emergency withdrawal function (admin only)*


```solidity
function emergencyWithdraw(address token, uint256 amount, address payable recipient) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The token to withdraw (address(0) for ETH)|
|`amount`|`uint256`|The amount to withdraw|
|`recipient`|`address payable`|The recipient address|


### getExerciseConfiguration

*Get exercise configuration*


```solidity
function getExerciseConfiguration()
    external
    view
    returns (address attestationRegistry, address targetShareClass, bytes32 valuationSchemaUID);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`attestationRegistry`|`address`|The attestation registry address|
|`targetShareClass`|`address`|The target share class address|
|`valuationSchemaUID`|`bytes32`|The valuation schema UID|


### receive

*Allow the facet to receive ETH for exercise payments*


```solidity
receive() external payable;
```

### _requireAdmin

*Internal function to check admin role using unified access control*


```solidity
function _requireAdmin() internal view;
```

### _getAuthority

*Get the authority address from the diamond*


```solidity
function _getAuthority() internal view returns (address);
```

## Events
### ExerciseConfigured

```solidity
event ExerciseConfigured(
    address indexed attestationRegistry, address indexed targetShareClass, bytes32 valuationSchemaUID
);
```

### TokenExercised

```solidity
event TokenExercised(
    bytes32 indexed tokenId,
    address indexed holder,
    uint96 exercisedQuantity,
    uint256 totalCost,
    address paymentCurrency,
    uint96 strikePrice
);
```

### PartialExercise

```solidity
event PartialExercise(
    bytes32 indexed tokenId, address indexed holder, uint96 exercisedQuantity, uint96 remainingQuantity
);
```

### ValuationUsed

```solidity
event ValuationUsed(
    address indexed company, uint256 valuationValue, uint256 valuationTimestamp, bytes32 attestationUID
);
```

## Structs
### ExerciseStorage

```solidity
struct ExerciseStorage {
    IAttestationRegistry attestationRegistry;
    IShareClass targetShareClass;
    bytes32 valuationSchemaUID;
    mapping(bytes32 => uint256) partialExercises;
}
```

