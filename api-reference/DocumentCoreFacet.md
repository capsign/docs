# DocumentCoreFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Documents/registry/facets/DocumentCoreFacet.sol)

Handles basic document registration and retrieval

*Core facet for DocumentRegistry diamond*


## Functions
### registerDocument

*Registers a new document entry*


```solidity
function registerDocument(bytes32 hash, string calldata uri, bool isTemplate, bytes32 templateHash)
    external
    returns (bytes32);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`hash`|`bytes32`|The hash of the document|
|`uri`|`string`|The URI pointing to the document location|
|`isTemplate`|`bool`|Whether this is a template|
|`templateHash`|`bytes32`|If not a template, the hash of template used|


### getDocument

*Gets document details*


```solidity
function getDocument(bytes32 hash, address owner)
    external
    view
    returns (
        bytes32 documentHash,
        string memory uri,
        address documentOwner,
        uint256 timestamp,
        bool isActive,
        bool isTemplate,
        bytes32 templateHash,
        IDocumentRegistry.SigningStatus status,
        uint256 expirationTime
    );
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`hash`|`bytes32`|The document hash|
|`owner`|`address`|The document owner|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`documentHash`|`bytes32`|The document hash|
|`uri`|`string`|The document URI|
|`documentOwner`|`address`|The document owner|
|`timestamp`|`uint256`|The document timestamp|
|`isActive`|`bool`|Whether the document is active|
|`isTemplate`|`bool`|Whether the document is a template|
|`templateHash`|`bytes32`|The template hash|
|`status`|`IDocumentRegistry.SigningStatus`||
|`expirationTime`|`uint256`||


### getDocumentsByOwner

*Gets all document hashes owned by an address*


```solidity
function getDocumentsByOwner(address owner) external view returns (bytes32[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|The address to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32[]`|Array of document hashes|


### deactivateDocument

*Deactivate a document*


```solidity
function deactivateDocument(bytes32 hash) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`hash`|`bytes32`|The document hash|


### reactivateDocument

*Reactivate a document*


```solidity
function reactivateDocument(bytes32 hash) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`hash`|`bytes32`|The document hash|


### documentExists

*Check if a document exists*


```solidity
function documentExists(bytes32 hash, address owner) external view returns (bool exists);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`hash`|`bytes32`|The document hash|
|`owner`|`address`|The document owner|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`exists`|`bool`|Whether the document exists|


## Events
### DocumentRegistered

```solidity
event DocumentRegistered(
    bytes32 indexed hash, string uri, address indexed owner, bool isTemplate, bytes32 templateHash
);
```

### DocumentDeactivated

```solidity
event DocumentDeactivated(bytes32 indexed hash, address indexed owner);
```

### DocumentReactivated

```solidity
event DocumentReactivated(bytes32 indexed hash, address indexed owner);
```

