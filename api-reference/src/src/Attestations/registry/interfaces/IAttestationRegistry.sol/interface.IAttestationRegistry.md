# IAttestationRegistry
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Attestations/registry/interfaces/IAttestationRegistry.sol)

*Interface for the AttestationRegistry diamond contract*


## Functions
### registerSchema


```solidity
function registerSchema(
    string calldata name,
    bytes32 easUID,
    bool revocable,
    uint64 expirationPeriod,
    bool isTimeSeries,
    string[] calldata fieldNames,
    string[] calldata fieldTypes
) external;
```

### deregisterSchema


```solidity
function deregisterSchema(bytes32 easUID) external;
```

### reactivateSchema


```solidity
function reactivateSchema(bytes32 easUID) external;
```

### setAttestationPaymentAmount


```solidity
function setAttestationPaymentAmount(bytes32 easUID, uint256 amount) external;
```

### requestAttestation


```solidity
function requestAttestation(bytes32 easUID) external payable returns (bool);
```

### attestationPaymentAmounts


```solidity
function attestationPaymentAmounts(bytes32 easUID) external view returns (uint256);
```

### createAttestation


```solidity
function createAttestation(
    bytes32 easUID,
    address subject,
    bytes calldata data,
    uint64 expirationOverride,
    address attestor,
    uint64 deadline,
    bytes calldata signature
) external payable returns (bytes32);
```

### updateAttestation


```solidity
function updateAttestation(
    bytes32 easUID,
    address subject,
    bytes calldata data,
    uint64 expirationOverride,
    address attestor,
    uint64 deadline,
    bytes calldata signature
) external payable returns (bytes32);
```

### revokeAttestation


```solidity
function revokeAttestation(bytes32 easUID, address subject, address attestor, uint64 deadline, bytes calldata signature)
    external;
```

### attestByDelegation


```solidity
function attestByDelegation(DelegatedProxyAttestationRequest calldata delegatedRequest)
    external
    payable
    returns (bytes32);
```

### revokeByDelegation


```solidity
function revokeByDelegation(DelegatedProxyRevocationRequest calldata delegatedRequest) external payable;
```

### setProxyMode


```solidity
function setProxyMode(bool enabled) external;
```

### isProxyModeEnabled


```solidity
function isProxyModeEnabled() external view returns (bool);
```

### getWebAuthnProxy


```solidity
function getWebAuthnProxy() external view returns (address);
```

### verifySignature


```solidity
function verifySignature(bytes32 hash, bytes memory signature, address expectedSigner) external view returns (bool);
```

### verifyWebAuthnSignature


```solidity
function verifyWebAuthnSignature(bytes32 hash, bytes memory signature, address expectedSigner)
    external
    view
    returns (bool);
```

### getAttestation


```solidity
function getAttestation(bytes32 easUID, address subject) external view returns (Attestation memory);
```

### getAttestationAtIndex


```solidity
function getAttestationAtIndex(bytes32 easUID, address subject, uint256 index)
    external
    view
    returns (Attestation memory);
```

### getAttestationHistory


```solidity
function getAttestationHistory(bytes32 easUID, address subject)
    external
    view
    returns (bytes32[] memory uids, uint256[] memory timestamps, bool[] memory activeFlags);
```

### getAttestationByName


```solidity
function getAttestationByName(string calldata schemaName, address subject) external view returns (Attestation memory);
```

### getDecodedAttestation


```solidity
function getDecodedAttestation(bytes32 easUID, address subject) external view returns (bytes[] memory values);
```

### getDecodedAttestationAtIndex


```solidity
function getDecodedAttestationAtIndex(bytes32 easUID, address subject, uint256 index)
    external
    view
    returns (bytes[] memory values);
```

### getAttestationHistoryCount


```solidity
function getAttestationHistoryCount(bytes32 easUID, address subject) external view returns (uint256);
```

### getLatestAttestationIndex


```solidity
function getLatestAttestationIndex(bytes32 easUID, address subject) external view returns (uint256);
```

### attestationRecords


```solidity
function attestationRecords(address subject, bytes32 easUID) external view returns (bytes32);
```

### attestationAttester


```solidity
function attestationAttester(bytes32 attestationUID) external view returns (address);
```

### validAttestors


```solidity
function validAttestors(address entity, address attestor) external view returns (bool);
```

### escrowPayments


```solidity
function escrowPayments(address subject, bytes32 easUID) external view returns (uint256);
```

### nonces


```solidity
function nonces(address addr) external view returns (uint256);
```

### authorizedDelegators


```solidity
function authorizedDelegators(address delegator) external view returns (bool);
```

### eas


```solidity
function eas() external view returns (IEAS);
```

### setValidAttestor


```solidity
function setValidAttestor(address attestor, bool isValid) external;
```

### isAttestationValid


```solidity
function isAttestationValid(address subject, bytes32 easUID, address entity) external view returns (bool);
```

### isAttestationValidByName


```solidity
function isAttestationValidByName(address subject, string calldata schemaName, address entity)
    external
    view
    returns (bool);
```

### initialize


```solidity
function initialize(address easAddress, address schemaRegistryAddress) external;
```

### setEAS


```solidity
function setEAS(address easAddress) external;
```

### setSchemaRegistry


```solidity
function setSchemaRegistry(address schemaRegistryAddress) external;
```

### withdrawAttestationPayment


```solidity
function withdrawAttestationPayment(address subject, bytes32 easUID) external;
```

### emergencyWithdraw


```solidity
function emergencyWithdraw(address payable recipient) external;
```

### schemaRegistry


```solidity
function schemaRegistry() external view returns (ISchemaRegistry);
```

### getBalance


```solidity
function getBalance() external view returns (uint256);
```

### batchAuthorizeDelegators


```solidity
function batchAuthorizeDelegators(address[] calldata delegators, bool[] calldata authorized) external;
```

### batchSetValidAttestors


```solidity
function batchSetValidAttestors(address[] calldata attestors, bool[] calldata validStatus) external;
```

### updateWebAuthnProxy


```solidity
function updateWebAuthnProxy(address newProxyAddress) external;
```

### pauseSchema


```solidity
function pauseSchema(bytes32 easUID, bool paused) external;
```

### authorizeDelegator


```solidity
function authorizeDelegator(address delegator) external;
```

### deauthorizeDelegator


```solidity
function deauthorizeDelegator(address delegator) external;
```

### isAuthorizedDelegator


```solidity
function isAuthorizedDelegator(address delegator) external view returns (bool);
```

### isAttestorTrustedByEntity


```solidity
function isAttestorTrustedByEntity(address entity, address attestor) external view returns (bool);
```

### isGloballyAuthorizedAttestor


```solidity
function isGloballyAuthorizedAttestor(address attestor) external view returns (bool);
```

### schemaNameToUID


```solidity
function schemaNameToUID(string calldata name) external view returns (bytes32);
```

### schemas


```solidity
function schemas(bytes32 easUID) external view returns (Schema memory);
```

### decodeSignatureWrapper


```solidity
function decodeSignatureWrapper(bytes calldata signature)
    external
    pure
    returns (uint8 sigType, bytes memory signatureData, bytes memory ownerData);
```

### decodeWebAuthnAuth


```solidity
function decodeWebAuthnAuth(bytes memory data) external pure returns (bytes memory);
```

## Events
### SchemaRegistered

```solidity
event SchemaRegistered(bytes32 indexed easUID, string name, bool isTimeSeries);
```

### SchemaUpdated

```solidity
event SchemaUpdated(bytes32 indexed easUID, string name, bool active);
```

### AttestationCreated

```solidity
event AttestationCreated(
    address indexed subject, bytes32 indexed easUID, bytes32 indexed attestationUID, uint256 timestamp
);
```

### AttestationRevoked

```solidity
event AttestationRevoked(address indexed subject, bytes32 indexed easUID, bytes32 indexed attestationUID);
```

### AttestationUpdated

```solidity
event AttestationUpdated(address indexed subject, bytes32 indexed easUID, bytes32 oldUID, bytes32 newUID);
```

### ValidAttestorSet

```solidity
event ValidAttestorSet(address indexed entity, address indexed attestor, bool isValid);
```

### EASUpdated

```solidity
event EASUpdated(address indexed oldEAS, address indexed newEAS);
```

### AttestationPaymentReceived

```solidity
event AttestationPaymentReceived(address indexed subject, bytes32 indexed easUID, uint256 amount);
```

### AttestationPaymentAmountSet

```solidity
event AttestationPaymentAmountSet(bytes32 indexed easUID, uint256 amount);
```

### AttestationRegistryDeployed

```solidity
event AttestationRegistryDeployed(
    address indexed registryAddress, address admin, address easAddress, address schemaRegistryAddress
);
```

### DelegatedAttestationCreated

```solidity
event DelegatedAttestationCreated(
    address indexed subject, bytes32 indexed easUID, bytes32 indexed attestationUID, address attestor, uint256 timestamp
);
```

### DelegatorAuthorized

```solidity
event DelegatorAuthorized(address indexed delegator);
```

### DelegatorDeauthorized

```solidity
event DelegatorDeauthorized(address indexed delegator);
```

### ProxyModeToggled

```solidity
event ProxyModeToggled(bool enabled);
```

## Structs
### Schema

```solidity
struct Schema {
    string name;
    bool revocable;
    uint64 expirationPeriod;
    bool active;
    bool isTimeSeries;
    string[] fieldNames;
    string[] fieldTypes;
}
```

### AttestationRecord

```solidity
struct AttestationRecord {
    bytes32 uid;
    uint256 timestamp;
    bool active;
}
```

