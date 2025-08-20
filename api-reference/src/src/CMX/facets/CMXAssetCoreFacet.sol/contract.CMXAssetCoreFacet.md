# CMXAssetCoreFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/CMX/facets/CMXAssetCoreFacet.sol)

Core asset queries and lot information for CMX token

*Adapted from IssuedAsset AssetCoreFacet for CMX token use*


## Functions
### onlyOwner


```solidity
modifier onlyOwner();
```

### lotIdToCustomId

Get custom ID from lot ID


```solidity
function lotIdToCustomId(bytes32 lotId) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID to look up|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The corresponding custom ID|


### customIdToLotId

Get lot ID from custom ID


```solidity
function customIdToLotId(uint256 customId) external view returns (bytes32);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`customId`|`uint256`|The custom ID to look up|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|The corresponding lot ID|


### lotExists

Check if a lot exists


```solidity
function lotExists(bytes32 lotId) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the lot exists|


### getLot

Get stored data for a specific lot


```solidity
function getLot(bytes32 lotId)
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
        CMXStorage.TransferType tType,
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
|`parentLotId`|`bytes32`|Hash of the parent lot, or 0x0 if none|
|`isValid`|`bool`|Whether the lot is active|
|`quantity`|`uint256`|The quantity|
|`paymentCurrency`|`address`|The payment currency for this lot|
|`costBasis`|`uint256`|The cost basis|
|`acquisitionDate`|`uint256`|The original acquisition timestamp|
|`lastUpdate`|`uint256`|The last time this lot was updated|
|`tType`|`CMXStorage.TransferType`|The transfer type|
|`uri`|`string`|The URI of the lot|
|`data`|`bytes`|Additional data associated with the lot|


### getLotsOf

Get all lot IDs owned by an address


```solidity
function getLotsOf(address account) external view returns (bytes32[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The owner address to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32[]`|Array of lot IDs owned by the address|


### getValidLotsOf

Get all valid lots owned by an address with details


```solidity
function getValidLotsOf(address account)
    external
    view
    returns (
        bytes32[] memory lotIds,
        uint256[] memory quantities,
        uint256[] memory costBases,
        uint256[] memory acquisitionDates
    );
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The owner address to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`lotIds`|`bytes32[]`|Array of valid lot IDs|
|`quantities`|`uint256[]`|Array of quantities for each lot|
|`costBases`|`uint256[]`|Array of cost bases for each lot|
|`acquisitionDates`|`uint256[]`|Array of acquisition dates for each lot|


### balanceOf

Get the total balance for an account


```solidity
function balanceOf(address account) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The account to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total balance|


### getNextCustomId

Get the next available custom ID


```solidity
function getNextCustomId() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The next custom ID that will be assigned|


### name

Get the token name


```solidity
function name() external view returns (string memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|The token name|


### symbol

Get the token symbol


```solidity
function symbol() external view returns (string memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|The token symbol|


### decimals

Get the token decimals


```solidity
function decimals() external view returns (uint8);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|The token decimals|


### totalSupply

Get the total supply


```solidity
function totalSupply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total supply|


### maxSupply

Get the maximum supply


```solidity
function maxSupply() external pure returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The maximum supply (1 billion CMX)|


### owner

Get the current owner


```solidity
function owner() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The owner address|


### transferOwnership

Transfer ownership of the CMX token


```solidity
function transferOwnership(address newOwner) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newOwner`|`address`|The new owner address|


### renounceOwnership

Renounce ownership of the CMX token


```solidity
function renounceOwnership() external onlyOwner;
```

### initialized

Check if the CMX token is initialized


```solidity
function initialized() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if initialized|


### isSourceChain

Check if this is a source chain


```solidity
function isSourceChain() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if source chain|


### lzEndpoint

Get the LayerZero endpoint address


```solidity
function lzEndpoint() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The endpoint address|


## Events
### OwnershipTransferred

```solidity
event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
```

## Errors
### NotOwner

```solidity
error NotOwner();
```

### InvalidAddress

```solidity
error InvalidAddress();
```

