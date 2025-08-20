# WebAuthnProxyFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Attestations/registry/facets/WebAuthnProxyFacet.sol)

**Inherits:**
[AccessManaged](/src/Authorization/AccessManaged.sol/abstract.AccessManaged.md)

*Facet for WebAuthn signature verification and EIP712 proxy delegation in the AttestationRegistry diamond*


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


### attestByDelegation

*Create attestation via WebAuthn proxy delegation*


```solidity
function attestByDelegation(DelegatedProxyAttestationRequest calldata delegatedRequest)
    external
    payable
    returns (bytes32);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`delegatedRequest`|`DelegatedProxyAttestationRequest`|The delegated attestation request|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|bytes32 The attestation UID|


### revokeByDelegation

*Revoke attestation via WebAuthn proxy delegation*


```solidity
function revokeByDelegation(DelegatedProxyRevocationRequest calldata delegatedRequest) external payable;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`delegatedRequest`|`DelegatedProxyRevocationRequest`|The delegated revocation request|


### setProxyMode

*Toggle proxy mode on/off (admin only)*


```solidity
function setProxyMode(bool enabled) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`enabled`|`bool`|Whether to enable proxy mode|


### isProxyModeEnabled

*Check if proxy mode is enabled*


```solidity
function isProxyModeEnabled() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if proxy mode is enabled|


### getWebAuthnProxy

*Get the WebAuthn proxy address*


```solidity
function getWebAuthnProxy() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|address The proxy contract address (this diamond itself)|


### verifySignature

*Verify either ECDSA or WebAuthn signature*


```solidity
function verifySignature(bytes32 hash, bytes memory signature, address expectedSigner) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`hash`|`bytes32`|The hash to verify|
|`signature`|`bytes`|The signature data (raw ECDSA bytes or wrapped signature)|
|`expectedSigner`|`address`|The expected signer address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if signature is valid|


### verifyWebAuthnSignature

*Verify WebAuthn signature and ensure the credential belongs to the smart account*


```solidity
function verifyWebAuthnSignature(bytes32 hash, bytes memory signature, address expectedSigner)
    external
    view
    returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`hash`|`bytes32`|The hash to verify|
|`signature`|`bytes`|The wrapped signature data containing WebAuthn signature and public key|
|`expectedSigner`|`address`|The expected signer address (smart account)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if signature is valid and credential belongs to the smart account|


### _verifyWebAuthnAttest

*Verifies delegated attestation request with WebAuthn support*


```solidity
function _verifyWebAuthnAttest(DelegatedProxyAttestationRequest memory request) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`request`|`DelegatedProxyAttestationRequest`|The arguments of the delegated attestation request|


### _verifyWebAuthnRevoke

*Verifies delegated revocation request with WebAuthn support*


```solidity
function _verifyWebAuthnRevoke(DelegatedProxyRevocationRequest memory request) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`request`|`DelegatedProxyRevocationRequest`|The arguments of the delegated revocation request|


### _verifyECDSA

*Verify ECDSA signature with safe error handling*


```solidity
function _verifyECDSA(bytes32 hash, bytes memory signature, address expectedSigner) internal pure returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`hash`|`bytes32`|The hash to verify|
|`signature`|`bytes`|The ECDSA signature (65 bytes: r + s + v)|
|`expectedSigner`|`address`|The expected signer address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if signature is valid|


### _tryWrappedSignature

*Try to verify wrapped signature (WebAuthn or other types)*


```solidity
function _tryWrappedSignature(bytes32 hash, bytes memory signature, address expectedSigner)
    internal
    view
    returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`hash`|`bytes32`|The hash to verify|
|`signature`|`bytes`|The wrapped signature data|
|`expectedSigner`|`address`|The expected signer address (smart account)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if signature is valid|


### _verifyWebAuthnSignature

*Verify WebAuthn signature and ensure the credential belongs to the smart account*


```solidity
function _verifyWebAuthnSignature(bytes32 hash, bytes memory signature, address expectedSigner)
    internal
    view
    returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`hash`|`bytes32`|The hash to verify|
|`signature`|`bytes`|The wrapped signature data containing WebAuthn signature and public key|
|`expectedSigner`|`address`|The expected signer address (smart account)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if signature is valid and credential belongs to the smart account|


### _verifyPublicKeyBelongsToAccount

*Verify that a public key belongs to a smart account*


```solidity
function _verifyPublicKeyBelongsToAccount(address smartAccount, uint256 pubKeyX, uint256 pubKeyY)
    internal
    view
    returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`smartAccount`|`address`|The smart account address|
|`pubKeyX`|`uint256`|The X coordinate of the public key|
|`pubKeyY`|`uint256`|The Y coordinate of the public key|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if the public key belongs to the smart account|


### _verifyWebAuthnSignatureData

*Verify the actual WebAuthn signature against the public key*


```solidity
function _verifyWebAuthnSignatureData(bytes32 hash, bytes memory signatureData, uint256 pubKeyX, uint256 pubKeyY)
    internal
    view
    returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`hash`|`bytes32`|The hash that was signed|
|`signatureData`|`bytes`|The WebAuthn signature data (should be WebAuthn.WebAuthnAuth struct)|
|`pubKeyX`|`uint256`|The X coordinate of the public key|
|`pubKeyY`|`uint256`|The Y coordinate of the public key|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if the signature is valid|


### _verifyUnusedSignature

*Verify signature hasn't been used before (prevents replay attacks)*


```solidity
function _verifyUnusedSignature(Signature memory signature) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`signature`|`Signature`|The signature to check|


### _hashTypedDataV4

*Get domain separator for EIP-712*


```solidity
function _hashTypedDataV4(bytes32 structHash) internal view returns (bytes32);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|bytes32 The domain separator|


### _getAttestProxyTypehash

*Get the attestation proxy typehash (should match EIP712Proxy)*


```solidity
function _getAttestProxyTypehash() internal pure returns (bytes32);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|bytes32 The typehash for attestation proxy|


### _getRevokeProxyTypehash

*Get the revocation proxy typehash (should match EIP712Proxy)*


```solidity
function _getRevokeProxyTypehash() internal pure returns (bytes32);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|bytes32 The typehash for revocation proxy|


### _verifySignature

*Verify signature using the main verifySignature function*


```solidity
function _verifySignature(bytes32 hash, bytes memory signature, address expectedSigner) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`hash`|`bytes32`|The hash to verify|
|`signature`|`bytes`|The signature|
|`expectedSigner`|`address`|The expected signer|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if signature is valid|


### decodeWebAuthnAuth

*External function to decode WebAuthn.WebAuthnAuth (used for try/catch)*


```solidity
function decodeWebAuthnAuth(bytes memory authData) external pure returns (WebAuthn.WebAuthnAuth memory auth);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`authData`|`bytes`|The encoded WebAuthn auth data|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`auth`|`WebAuthn.WebAuthnAuth`|The decoded WebAuthn auth struct|


## Events
### ProxyModeToggled

```solidity
event ProxyModeToggled(bool enabled);
```

