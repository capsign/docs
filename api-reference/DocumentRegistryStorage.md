# DocumentRegistryStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Documents/registry/storage/DocumentRegistryStorage.sol)

Contains all storage variables for document registry functionality

*Storage library for DocumentRegistry diamond*


## State Variables
### STORAGE_SLOT

```solidity
bytes32 internal constant STORAGE_SLOT = keccak256("capsign.storage.document_registry");
```


## Functions
### layout


```solidity
function layout() internal pure returns (Layout storage l);
```

## Structs
### Layout

```solidity
struct Layout {
    mapping(bytes32 => mapping(address => IDocumentRegistry.Document)) documents;
    mapping(address => bytes32[]) ownerDocuments;
    mapping(address => bytes32[]) pendingSignatures;
    mapping(bytes32 => address) documentOwners;
    mapping(bytes32 => bool) approvedTemplates;
    mapping(bytes32 => address) templateOwners;
    bytes32[] allTemplates;
    mapping(address => bool) registryAdmins;
    address owner;
    bool registrationPaused;
    uint256 maxDocumentSize;
    uint256 defaultExpirationPeriod;
    mapping(bytes32 => uint256) documentVersions;
    mapping(bytes32 => bytes32[]) documentHistory;
    mapping(bytes32 => bytes32[]) batchDocuments;
    uint256 nextBatchId;
    string registryName;
    bool isGlobal;
    address entity;
    address accessManager;
}
```

