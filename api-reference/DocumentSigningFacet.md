# DocumentSigningFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Documents/registry/facets/DocumentSigningFacet.sol)

Handles document signing workflows and signer management

*Signing facet for DocumentRegistry diamond*


## Functions
### addSigner

*Add a signer to a document*


```solidity
function addSigner(bytes32 documentHash, address signerAddress, uint256 signatureOrder, bool isRequired) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`documentHash`|`bytes32`|The hash of the document|
|`signerAddress`|`address`|The address of the signer|
|`signatureOrder`|`uint256`|Order number (0 for parallel)|
|`isRequired`|`bool`|Whether this signature is required|


### signDocument

*Sign a document*


```solidity
function signDocument(bytes32 documentHash, address owner) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`documentHash`|`bytes32`|The hash of the document to sign|
|`owner`|`address`|The owner of the document|


### rejectDocument

*Reject signing a document*


```solidity
function rejectDocument(bytes32 documentHash, address owner, string calldata reason) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`documentHash`|`bytes32`|The hash of the document|
|`owner`|`address`|The owner of the document|
|`reason`|`string`|Reason for rejection|


### getSignerDetails

*Get signer details for a document*


```solidity
function getSignerDetails(bytes32 hash, address owner, address signerAddress)
    external
    view
    returns (address account, bool hasSigned, uint256 signedAt, bool isRequired, uint256 signatureOrder);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`hash`|`bytes32`|The document hash|
|`owner`|`address`|The document owner|
|`signerAddress`|`address`|The signer address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The signer address|
|`hasSigned`|`bool`|Whether the signer has signed|
|`signedAt`|`uint256`|The timestamp of the signer's signature|
|`isRequired`|`bool`|Whether the signer is required|
|`signatureOrder`|`uint256`|The signature order|


### getPendingSignatures

*Get all pending signatures for an address*


```solidity
function getPendingSignatures(address signer) external view returns (bytes32[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`signer`|`address`|The signer address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32[]`|Array of document hashes pending signature|


### getDocumentSigners

*Get all signers for a document*


```solidity
function getDocumentSigners(bytes32 hash, address owner) external view returns (address[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`hash`|`bytes32`|The document hash|
|`owner`|`address`|The document owner|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Array of signer addresses|


### _validateSequentialOrder


```solidity
function _validateSequentialOrder(IDocumentRegistry.Document storage doc, address signer) internal view;
```

### _isSigningComplete


```solidity
function _isSigningComplete(IDocumentRegistry.Document storage doc) internal view returns (bool);
```

### _removePendingSignature


```solidity
function _removePendingSignature(DocumentRegistryStorage.Layout storage s, address signer, bytes32 documentHash)
    internal;
```

### _removeFromAllPendingSignatures


```solidity
function _removeFromAllPendingSignatures(
    DocumentRegistryStorage.Layout storage s,
    IDocumentRegistry.Document storage doc,
    bytes32 documentHash
) internal;
```

## Events
### SignerAdded

```solidity
event SignerAdded(bytes32 indexed documentHash, address indexed signer, uint256 signatureOrder, bool isRequired);
```

### DocumentSigned

```solidity
event DocumentSigned(bytes32 indexed documentHash, address indexed signer, uint256 timestamp);
```

### SigningCompleted

```solidity
event SigningCompleted(bytes32 indexed documentHash, address indexed owner);
```

### SigningRejected

```solidity
event SigningRejected(bytes32 indexed documentHash, address indexed signer, string reason);
```

