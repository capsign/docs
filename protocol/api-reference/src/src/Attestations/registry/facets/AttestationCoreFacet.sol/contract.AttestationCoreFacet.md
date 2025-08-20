# AttestationCoreFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Attestations/registry/facets/AttestationCoreFacet.sol)

**Inherits:**
[AccessManaged](/src/Authorization/AccessManaged.sol/abstract.AccessManaged.md)

*Facet for core attestation operations in the AttestationRegistry diamond*

*Now includes event bubbling to diamond level for subgraph indexing*


## Functions
### constructor

*Constructor*


```solidity
constructor(address _globalAccessManager) AccessManaged(_globalAccessManager);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_globalAccessManager`|`address`|Address of the GlobalAccessManager contract|


### createAttestation

*Create a new attestation*


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
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`easUID`|`bytes32`|The EAS UID of the schema|
|`subject`|`address`|The address to attest for|
|`data`|`bytes`|The attestation data|
|`expirationOverride`|`uint64`|Override the default expiration (0 = use default)|
|`attestor`|`address`|The address of the actual attestor|
|`deadline`|`uint64`|Deadline for the signature validity|
|`signature`|`bytes`|The attestor's signature (ECDSA or WebAuthn wrapped)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|bytes32 The attestation UID|


### updateAttestation

*Update an existing attestation (for time series schemas)*


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
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`easUID`|`bytes32`|The EAS UID of the schema|
|`subject`|`address`|The address to attest for|
|`data`|`bytes`|The new attestation data|
|`expirationOverride`|`uint64`|Override the default expiration (0 = use default)|
|`attestor`|`address`|The address of the actual attestor|
|`deadline`|`uint64`|Deadline for the signature validity|
|`signature`|`bytes`|The attestor's signature|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|bytes32 The new attestation UID|


### revokeAttestation

*Revoke an attestation*


```solidity
function revokeAttestation(bytes32 easUID, address subject, address attestor, uint64 deadline, bytes calldata signature)
    external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`easUID`|`bytes32`|The EAS UID of the schema|
|`subject`|`address`|The address whose attestation to revoke|
|`attestor`|`address`|The address of the attestor|
|`deadline`|`uint64`|Deadline for the signature validity|
|`signature`|`bytes`|The attestor's signature|


### requestAttestation

*Request an attestation with payment*


```solidity
function requestAttestation(bytes32 easUID) external payable returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`easUID`|`bytes32`|The EAS UID of the schema|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if payment was successful|


### _createAttestationHash

*Create hash for attestation signature verification*


```solidity
function _createAttestationHash(
    bytes32 easUID,
    address subject,
    bytes calldata data,
    uint64 expirationTime,
    address attestor,
    uint64 deadline
) internal view returns (bytes32);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`easUID`|`bytes32`|Schema UID|
|`subject`|`address`|Subject address|
|`data`|`bytes`|Attestation data|
|`expirationTime`|`uint64`|Expiration timestamp|
|`attestor`|`address`|Attestor address|
|`deadline`|`uint64`|Signature deadline|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|bytes32 The message hash|


### _createRevocationHash

*Create hash for revocation signature verification*


```solidity
function _createRevocationHash(bytes32 easUID, bytes32 attestationUID, address attestor, uint64 deadline)
    internal
    view
    returns (bytes32);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`easUID`|`bytes32`|Schema UID|
|`attestationUID`|`bytes32`|Attestation UID to revoke|
|`attestor`|`address`|Attestor address|
|`deadline`|`uint64`|Signature deadline|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|bytes32 The message hash|


### _domainSeparator

*Get domain separator for EIP-712*


```solidity
function _domainSeparator() internal view returns (bytes32);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|bytes32 The domain separator|


### _verifySignature

*Verify signature (ECDSA or WebAuthn)*


```solidity
function _verifySignature(bytes32 messageHash, bytes calldata signature, address expectedSigner)
    internal
    view
    returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`messageHash`|`bytes32`|The message hash|
|`signature`|`bytes`|The signature|
|`expectedSigner`|`address`|The expected signer|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if signature is valid|


### _verifyWrappedSignature

*Verify wrapped signature (WebAuthn or other types)*


```solidity
function _verifyWrappedSignature(bytes32 messageHash, bytes calldata signature, address expectedSigner)
    internal
    view
    returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`messageHash`|`bytes32`|The message hash|
|`signature`|`bytes`|The wrapped signature|
|`expectedSigner`|`address`|The expected signer|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if signature is valid|


### _verifyWebAuthnSignature

*Verify WebAuthn signature (placeholder - actual implementation in WebAuthnProxyFacet)*


```solidity
function _verifyWebAuthnSignature(bytes32, bytes calldata, address) internal pure returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if signature is valid|


### _isAuthorizedAttestor

*Check if an attestor is authorized for a schema*


```solidity
function _isAuthorizedAttestor(address attestor, bytes32) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`attestor`|`address`|The attestor address|
|`<none>`|`bytes32`||

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if authorized|


### decodeSignatureWrapper

*External function to decode SignatureWrapper (used for try/catch)*


```solidity
function decodeSignatureWrapper(bytes calldata signature)
    external
    pure
    returns (uint8 sigType, bytes memory signatureData, bytes memory ownerData);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`signature`|`bytes`|The encoded signature wrapper|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`sigType`|`uint8`|The signature type|
|`signatureData`|`bytes`|The signature data|
|`ownerData`|`bytes`|The owner data|


## Events
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

### AttestationPaymentReceived

```solidity
event AttestationPaymentReceived(address indexed subject, bytes32 indexed easUID, uint256 amount);
```

### DelegatedAttestationCreated

```solidity
event DelegatedAttestationCreated(
    address indexed subject, bytes32 indexed easUID, bytes32 indexed attestationUID, address attestor, uint256 timestamp
);
```

