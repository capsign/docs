# AttestationRegistryStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Attestations/registry/storage/AttestationRegistryStorage.sol)

Contains all storage variables from AttestationRegistry, AttestationRegistryWithProxy, and WebAuthnEIP712Proxy

*Storage contract for AttestationRegistry diamond*


## State Variables
### STORAGE_SLOT

```solidity
bytes32 internal constant STORAGE_SLOT = keccak256("capsign.storage.attestation_registry");
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
    mapping(bytes32 => IAttestationRegistry.Schema) schemas;
    mapping(string => bytes32) schemaNameToUID;
    mapping(address => mapping(bytes32 => bytes32)) attestationRecords;
    mapping(address => mapping(bytes32 => IAttestationRegistry.AttestationRecord[])) attestationHistory;
    mapping(address => mapping(bytes32 => uint256)) latestAttestationIndex;
    mapping(address => mapping(address => bool)) validAttestors;
    mapping(bytes32 => uint256) attestationPaymentAmounts;
    mapping(address => mapping(bytes32 => uint256)) escrowPayments;
    IEAS eas;
    ISchemaRegistry schemaRegistry;
    mapping(address => uint256) nonces;
    mapping(address => bool) authorizedDelegators;
    mapping(bytes32 => address) attestationAttester;
    bool proxyModeEnabled;
    mapping(bytes32 => bool) usedSignatures;
    string registryName;
    bool isGlobal;
    address entity;
    address accessManager;
}
```

