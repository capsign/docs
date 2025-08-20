# IDocumentRegistry
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Documents/registry/interfaces/IDocumentRegistry.sol)

Lightweight interface for the DocumentRegistry contract exposing
events, enums and external callable functions. Contracts that only
need to interact with the registry should import this interface
instead of the full implementation to reduce byte-code size.


## Functions
### registerDocument


```solidity
function registerDocument(bytes32 hash, string calldata uri, bool isTemplate, bytes32 templateHash)
    external
    returns (bytes32);
```

### addSigner


```solidity
function addSigner(bytes32 documentHash, address signerAddress, uint256 signatureOrder, bool isRequired) external;
```

### sign


```solidity
function sign(bytes32 documentHash, address owner) external;
```

### rejectDocument


```solidity
function rejectDocument(bytes32 documentHash) external;
```

### setDocumentExpiration


```solidity
function setDocumentExpiration(bytes32 documentHash, uint256 expirationTime) external;
```

### updateDocumentURI


```solidity
function updateDocumentURI(bytes32 hash, string calldata newUri) external;
```

### deactivateDocument


```solidity
function deactivateDocument(bytes32 hash) external;
```

### reactivateDocument


```solidity
function reactivateDocument(bytes32 hash) external;
```

### documentOwners


```solidity
function documentOwners(bytes32 hash) external view returns (address);
```

### documents


```solidity
function documents(bytes32 hash, address owner)
    external
    view
    returns (
        bytes32 docHash,
        string memory uri,
        address docOwner,
        uint256 timestamp,
        bool isActive,
        bool isTemplate,
        bytes32 templateHash,
        uint256 requiredSignatures,
        bool isSequential,
        SigningStatus status,
        uint256 expirationTime
    );
```

### getDocumentInfo


```solidity
function getDocumentInfo(bytes32 hash)
    external
    view
    returns (
        bytes32 docHash,
        string memory uri,
        address owner,
        uint256 timestamp,
        bool isActive,
        bool isTemplate,
        bytes32 templateHash,
        uint256 requiredSignatures,
        bool isSequential,
        SigningStatus status,
        uint256 expirationTime
    );
```

### getDocumentSigners


```solidity
function getDocumentSigners(bytes32 hash) external view returns (address[] memory);
```

### getSignerDetails


```solidity
function getSignerDetails(bytes32 hash, address signerAddress)
    external
    view
    returns (address account, bool hasSigned, uint256 signedAt, bool isRequired, uint256 signatureOrder);
```

### getSignerDetailsByOwner


```solidity
function getSignerDetailsByOwner(bytes32 hash, address owner, address signerAddress)
    external
    view
    returns (address account, bool hasSigned, uint256 signedAt, bool isRequired, uint256 signatureOrder);
```

### getDocumentsByOwner


```solidity
function getDocumentsByOwner(address owner) external view returns (bytes32[] memory);
```

### getPendingSignatures


```solidity
function getPendingSignatures(address signer) external view returns (bytes32[] memory);
```

### registerInstanceAndSign


```solidity
function registerInstanceAndSign(bytes32 templateHash, string calldata uri) external returns (bytes32 instanceHash);
```

## Events
### DocumentRegistryCreated

```solidity
event DocumentRegistryCreated(address indexed registry);
```

### DocumentRegistered

```solidity
event DocumentRegistered(
    bytes32 indexed hash, string uri, address indexed owner, bool isTemplate, bytes32 templateHash
);
```

### SignerAdded

```solidity
event SignerAdded(bytes32 indexed documentHash, address indexed signer, uint256 signatureOrder, bool isRequired);
```

### DocumentSigned

```solidity
event DocumentSigned(bytes32 indexed documentHash, address indexed signer, uint256 timestamp);
```

### DocumentRejected

```solidity
event DocumentRejected(bytes32 indexed documentHash, address indexed signer, uint256 timestamp);
```

### DocumentURIUpdated

```solidity
event DocumentURIUpdated(bytes32 indexed hash, string oldUri, string newUri);
```

### DocumentDeactivated

```solidity
event DocumentDeactivated(bytes32 indexed hash);
```

### DocumentReactivated

```solidity
event DocumentReactivated(bytes32 indexed hash);
```

### DocumentStatusChanged

```solidity
event DocumentStatusChanged(bytes32 indexed hash, SigningStatus previousStatus, SigningStatus newStatus);
```

## Structs
### Signer

```solidity
struct Signer {
    address account;
    bool hasSigned;
    uint256 signedAt;
    bool isRequired;
    uint256 signatureOrder;
}
```

### Document

```solidity
struct Document {
    bytes32 hash;
    string uri;
    address owner;
    uint256 timestamp;
    bool isActive;
    bool isTemplate;
    bytes32 templateHash;
    address[] signers;
    mapping(address => Signer) signerDetails;
    uint256 requiredSignatures;
    bool isSequential;
    SigningStatus status;
    uint256 expirationTime;
}
```

## Enums
### SigningStatus

```solidity
enum SigningStatus {
    Pending,
    Completed,
    Rejected,
    Expired
}
```

