# AssetCoreFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/facets/AssetCoreFacet.sol)

Core functionality for IssuedAsset including initialization and basic getters

*Diamond facet providing basic asset functionality*


## Functions
### initialize

Initialize the IssuedAsset with  information


```solidity
function initialize(address _entity, string memory _name, string memory _symbol, string memory _baseURI) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_entity`|`address`|The entity of the asset|
|`_name`|`string`|The name of the asset|
|`_symbol`|`string`|The symbol for the asset|
|`_baseURI`|`string`|Optional base URI for asset-level metadata|


### name

Returns the name of the asset


```solidity
function name() external view returns (string memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|The name of the asset|


### symbol

Returns the symbol of the asset


```solidity
function symbol() external view returns (string memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|The symbol of the asset|


### decimals

Returns the decimals of the asset


```solidity
function decimals() external view returns (uint8);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|The decimals of the asset|


### totalSupply

Returns the total supply of the asset


```solidity
function totalSupply() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total supply of the asset|


### baseURI

Returns the base URI for asset metadata


```solidity
function baseURI() external view returns (string memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|The base URI string|


### balanceOf

Returns the balance of a specific address


```solidity
function balanceOf(address account) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The balance of the address|


### owner

Returns the owner of the asset


```solidity
function owner() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The owner address|


### paused

Returns whether the contract is paused


```solidity
function paused() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if paused, false otherwise|


### nextCustomId

Returns the next custom ID that will be assigned


```solidity
function nextCustomId() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The next custom ID|


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
|`parentLotId`|`bytes32`|Hash of the parent lot, or 0x0 if none|
|`isValid`|`bool`|Whether the lot is active|
|`quantity`|`uint256`|The quantity|
|`paymentCurrency`|`address`|The payment currency for this lot|
|`costBasis`|`uint256`|The cost basis|
|`acquisitionDate`|`uint256`|The original acquisition timestamp|
|`lastUpdate`|`uint256`|The last time this lot was updated|
|`tType`|`IIssuedAsset.TransferType`|The transfer type|
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


### transferOwnership

Transfer ownership of the asset


```solidity
function transferOwnership(address newOwner) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newOwner`|`address`|The new owner address|


### renounceOwnership

Renounce ownership of the asset


```solidity
function renounceOwnership() external;
```

### setBaseURI

Set the base URI for the asset


```solidity
function setBaseURI(string memory newBaseURI) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newBaseURI`|`string`|The new base URI string|


## Events
### OwnershipTransferred

```solidity
event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
```

### BaseURIUpdated

```solidity
event BaseURIUpdated(string newBaseURI);
```

