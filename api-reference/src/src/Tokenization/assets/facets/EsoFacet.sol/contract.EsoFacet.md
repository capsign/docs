# EsoFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/facets/EsoFacet.sol)

Facet providing Employee Stock Option functionality

*Diamond facet for ESO creation, vesting, exercise, and management*


## Functions
### initializeEso

Initialize the ESO functionality


```solidity
function initializeEso() external;
```

### setCompensationIntegration

Set the compensation factory and Rule701 diamond for this ESO


```solidity
function setCompensationIntegration(address compensationFactory, address rule701Diamond) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`compensationFactory`|`address`|Address of the compensation factory|
|`rule701Diamond`|`address`|Address of the entity's Rule701 diamond (optional)|


### createOptionToken

Create a new option token with vesting parameters


```solidity
function createOptionToken(
    address owner,
    uint96 quantity,
    address paymentCurrency,
    uint96 strikePrice,
    uint256 vestingStartTime,
    uint256 cliffTime,
    uint256 vestingEndTime
) external returns (bytes32 tokenId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The employee who will own the options|
|`quantity`|`uint96`|The number of options granted|
|`paymentCurrency`|`address`|The currency in which the options are denominated|
|`strikePrice`|`uint96`|The price per share|
|`vestingStartTime`|`uint256`|The timestamp when vesting begins|
|`cliffTime`|`uint256`|The timestamp when the cliff ends|
|`vestingEndTime`|`uint256`|The timestamp when all options are fully vested|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The unique identifier of this option token|


### createOptionTokensBatch

Create multiple option tokens in batch


```solidity
function createOptionTokensBatch(
    address[] memory owners,
    uint96[] memory quantities,
    address[] memory paymentCurrencies,
    uint96[] memory strikePrices,
    uint256[] memory vestingStartTimes,
    uint256[] memory cliffTimes,
    uint256[] memory vestingEndTimes
) external returns (bytes32[] memory tokenIds);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owners`|`address[]`|Array of employee addresses|
|`quantities`|`uint96[]`|Array of quantities|
|`paymentCurrencies`|`address[]`|Array of payment currencies|
|`strikePrices`|`uint96[]`|Array of strike prices|
|`vestingStartTimes`|`uint256[]`|Array of vesting start times|
|`cliffTimes`|`uint256[]`|Array of cliff times|
|`vestingEndTimes`|`uint256[]`|Array of vesting end times|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tokenIds`|`bytes32[]`|Array of created token IDs|


### invalidateOptionToken

Invalidate an option token


```solidity
function invalidateOptionToken(bytes32 tokenId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID to invalidate|


### exerciseOption

Exercise options (by the owner)


```solidity
function exerciseOption(bytes32 tokenId, uint96 exerciseQuantity) external returns (uint96 remainingQuantity);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID to exercise|
|`exerciseQuantity`|`uint96`|The quantity of options to exercise|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`remainingQuantity`|`uint96`|The remaining quantity after exercise|


### exerciseOptionFor

Exercise options on behalf of the owner (by admin)


```solidity
function exerciseOptionFor(bytes32 tokenId, uint96 exerciseQuantity, address exerciser)
    external
    returns (uint96 remainingQuantity);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID to exercise|
|`exerciseQuantity`|`uint96`|The quantity of options to exercise|
|`exerciser`|`address`|The address of the person exercising (must be the owner)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`remainingQuantity`|`uint96`|The remaining quantity after exercise|


### _exerciseOptionInternal

Internal function to exercise options with vesting check


```solidity
function _exerciseOptionInternal(bytes32 tokenId, uint96 exerciseQuantity, address exerciser)
    internal
    returns (uint96 remainingQuantity);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID to exercise|
|`exerciseQuantity`|`uint96`|The quantity to exercise|
|`exerciser`|`address`|The address exercising the options|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`remainingQuantity`|`uint96`|The remaining quantity after exercise|


### transferOptionToken

Transfer an option token to a new owner


```solidity
function transferOptionToken(bytes32 tokenId, address newOwner) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID to transfer|
|`newOwner`|`address`|The new owner address|


### getOptionToken

Get option token information


```solidity
function getOptionToken(bytes32 tokenId)
    external
    view
    returns (
        bool isValid,
        address owner,
        uint96 quantity,
        address paymentCurrency,
        uint96 strikePrice,
        uint96 totalGrantAmount,
        uint256 vestingStartTime,
        uint256 cliffTime,
        uint256 vestingEndTime
    );
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
|`quantity`|`uint96`|The current quantity|
|`paymentCurrency`|`address`|The payment currency|
|`strikePrice`|`uint96`|The strike price|
|`totalGrantAmount`|`uint96`|The original grant amount|
|`vestingStartTime`|`uint256`|The vesting start time|
|`cliffTime`|`uint256`|The cliff time|
|`vestingEndTime`|`uint256`|The vesting end time|


### isOptionTokenValid

Check if an option token is valid


```solidity
function isOptionTokenValid(bytes32 tokenId) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if valid|


### getOwnerOptions

Get all option tokens for an owner


```solidity
function getOwnerOptions(address owner) external view returns (bytes32[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32[]`|Array of token IDs|


### getOwnerOptionCount

Get the number of option tokens for an owner


```solidity
function getOwnerOptionCount(address owner) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The count of option tokens|


### getOwnerTotalQuantity

Get total quantity of options for an owner


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


### getOwnerTotalVestedQuantity

Get total vested quantity for an owner


```solidity
function getOwnerTotalVestedQuantity(address owner) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The owner address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total vested quantity|


### calculateVestedQuantity

Calculate vested quantity for an option token


```solidity
function calculateVestedQuantity(bytes32 tokenId) external view returns (uint96);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint96`|The vested quantity as of current time|


### calculateVestedQuantityAtTime

Calculate vested quantity at a specific time


```solidity
function calculateVestedQuantityAtTime(bytes32 tokenId, uint256 timestamp) external view returns (uint96);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`bytes32`|The token ID|
|`timestamp`|`uint256`|The timestamp to calculate vesting for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint96`|The vested quantity at the specified time|


### calculateExerciseValue

Calculate exercise value for an option token


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


### isInTheMoney

Check if option is in the money


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


### getOwnerOptionDetails

Get detailed option information for an owner


```solidity
function getOwnerOptionDetails(address owner)
    external
    view
    returns (
        bytes32[] memory tokenIds,
        uint96[] memory quantities,
        uint96[] memory totalGrantAmounts,
        uint96[] memory strikePrices,
        uint256[] memory vestingStartTimes,
        uint256[] memory cliffTimes,
        uint256[] memory vestingEndTimes,
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
|`quantities`|`uint96[]`|Array of current quantities|
|`totalGrantAmounts`|`uint96[]`|Array of original grant amounts|
|`strikePrices`|`uint96[]`|Array of strike prices|
|`vestingStartTimes`|`uint256[]`|Array of vesting start times|
|`cliffTimes`|`uint256[]`|Array of cliff times|
|`vestingEndTimes`|`uint256[]`|Array of vesting end times|
|`validFlags`|`bool[]`|Array of validity flags|


### getVestingInfoBatch

Get vesting information for multiple option tokens


```solidity
function getVestingInfoBatch(bytes32[] memory tokenIds)
    external
    view
    returns (uint96[] memory vestedQuantities, uint96[] memory totalGrantAmounts, uint256[] memory vestingPercentages);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenIds`|`bytes32[]`|Array of token IDs|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`vestedQuantities`|`uint96[]`|Array of vested quantities|
|`totalGrantAmounts`|`uint96[]`|Array of total grant amounts|
|`vestingPercentages`|`uint256[]`|Array of vesting percentages (in basis points)|


### getOwnerOptionSummary

Get summary statistics for all options of an owner


```solidity
function getOwnerOptionSummary(address owner, uint96 currentPrice)
    external
    view
    returns (
        uint256 totalQuantity,
        uint256 totalVestedQuantity,
        uint256 totalExerciseValue,
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
|`totalQuantity`|`uint256`|Total quantity of valid options|
|`totalVestedQuantity`|`uint256`|Total vested quantity|
|`totalExerciseValue`|`uint256`|Total exercise value at current price|
|`inTheMoneyCount`|`uint256`|Number of options that are in the money|
|`validTokenCount`|`uint256`|Number of valid option tokens|
|`totalTokenCount`|`uint256`|Total number of option tokens (including invalid)|


### isEsoInitialized

Check if ESO is initialized


```solidity
function isEsoInitialized() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if initialized|


### getOptionTokenWithVesting

Get option token with vesting and exercise information


```solidity
function getOptionTokenWithVesting(bytes32 tokenId, uint96 currentPrice)
    external
    view
    returns (
        bool isValid,
        address owner,
        uint96 quantity,
        uint96 strikePrice,
        uint96 totalGrantAmount,
        uint96 vestedQuantity,
        uint256 vestingPercentage,
        uint256 exerciseValue,
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
|`quantity`|`uint96`|The current quantity|
|`strikePrice`|`uint96`|The strike price|
|`totalGrantAmount`|`uint96`|The original grant amount|
|`vestedQuantity`|`uint96`|The current vested quantity|
|`vestingPercentage`|`uint256`|The vesting percentage (in basis points)|
|`exerciseValue`|`uint256`|The current exercise value|
|`inTheMoney`|`bool`|Whether the option is in the money|


## Events
### OptionCreated

```solidity
event OptionCreated(
    bytes32 indexed tokenId,
    address indexed owner,
    uint96 quantity,
    address paymentCurrency,
    uint96 strikePrice,
    uint256 vestingStartTime,
    uint256 cliffTime,
    uint256 vestingEndTime
);
```

### OptionInvalidated

```solidity
event OptionInvalidated(bytes32 indexed tokenId);
```

### OptionExercised

```solidity
event OptionExercised(
    bytes32 indexed tokenId, uint96 exercisedQuantity, uint96 remainingQuantity, address indexed exerciser
);
```

### OptionTransferred

```solidity
event OptionTransferred(bytes32 indexed tokenId, address indexed from, address indexed to);
```

