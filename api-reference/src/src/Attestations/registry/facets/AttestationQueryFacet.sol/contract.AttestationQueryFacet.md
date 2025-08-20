# AttestationQueryFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Attestations/registry/facets/AttestationQueryFacet.sol)

*Facet for querying attestations in the AttestationRegistry diamond*


## Functions
### getAttestation

*Get an attestation by schema UID and subject*


```solidity
function getAttestation(bytes32 easUID, address subject) external view returns (Attestation memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`easUID`|`bytes32`|The EAS schema UID|
|`subject`|`address`|The subject address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`Attestation`|Attestation The attestation with correct attester|


### getAttestationAtIndex

*Get an attestation at a specific index in the history*


```solidity
function getAttestationAtIndex(bytes32 easUID, address subject, uint256 index)
    external
    view
    returns (Attestation memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`easUID`|`bytes32`|The EAS schema UID|
|`subject`|`address`|The subject address|
|`index`|`uint256`|The index in the attestation history|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`Attestation`|Attestation The attestation at the specified index|


### getAttestationHistory

*Get the full attestation history for a subject and schema*


```solidity
function getAttestationHistory(bytes32 easUID, address subject)
    external
    view
    returns (bytes32[] memory uids, uint256[] memory timestamps, bool[] memory activeFlags);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`easUID`|`bytes32`|The EAS schema UID|
|`subject`|`address`|The subject address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`uids`|`bytes32[]`|Array of attestation UIDs|
|`timestamps`|`uint256[]`|Array of creation timestamps|
|`activeFlags`|`bool[]`|Array of active status flags|


### getAttestationByName

*Get an attestation by schema name and subject*


```solidity
function getAttestationByName(string calldata schemaName, address subject) external view returns (Attestation memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`schemaName`|`string`|The human-readable schema name|
|`subject`|`address`|The subject address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`Attestation`|Attestation The attestation|


### getDecodedAttestation

*Get decoded attestation data as an array of values*


```solidity
function getDecodedAttestation(bytes32 easUID, address subject) external view returns (bytes[] memory values);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`easUID`|`bytes32`|The EAS schema UID|
|`subject`|`address`|The subject address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`values`|`bytes[]`|Array of decoded values|


### getDecodedAttestationAtIndex

*Get decoded attestation data at a specific index*


```solidity
function getDecodedAttestationAtIndex(bytes32 easUID, address subject, uint256 index)
    external
    view
    returns (bytes[] memory values);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`easUID`|`bytes32`|The EAS schema UID|
|`subject`|`address`|The subject address|
|`index`|`uint256`|The index in the attestation history|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`values`|`bytes[]`|Array of decoded values|


### isAttestationValid

*Check if an attestation is valid for a subject and entity*


```solidity
function isAttestationValid(address subject, bytes32 easUID, address entity) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subject`|`address`|The subject address|
|`easUID`|`bytes32`|The EAS schema UID|
|`entity`|`address`|The entity address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if the attestation is valid|


### isAttestationValidByName

*Check if an attestation is valid by schema name*


```solidity
function isAttestationValidByName(address subject, string calldata schemaName, address entity)
    external
    view
    returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subject`|`address`|The subject address|
|`schemaName`|`string`|The schema name|
|`entity`|`address`|The entity address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if the attestation is valid|


### getAttestationHistoryCount

*Get the count of attestations in history for a subject and schema*


```solidity
function getAttestationHistoryCount(bytes32 easUID, address subject) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`easUID`|`bytes32`|The EAS schema UID|
|`subject`|`address`|The subject address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 The count of attestations|


### getLatestAttestationIndex

*Get the latest active attestation index for a subject and schema*


```solidity
function getLatestAttestationIndex(bytes32 easUID, address subject) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`easUID`|`bytes32`|The EAS schema UID|
|`subject`|`address`|The subject address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 The latest attestation index|


### attestationRecords

*Get attestation records mapping (for single attestations)*


```solidity
function attestationRecords(address subject, bytes32 easUID) external view returns (bytes32);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subject`|`address`|The subject address|
|`easUID`|`bytes32`|The EAS schema UID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|bytes32 The attestation UID|


### attestationAttester

*Get attestation attester mapping*


```solidity
function attestationAttester(bytes32 attestationUID) external view returns (address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`attestationUID`|`bytes32`|The attestation UID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|address The real attester address|


### validAttestors

*Check if an attestor is valid for an entity*


```solidity
function validAttestors(address entity, address attestor) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|The entity address|
|`attestor`|`address`|The attestor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if the attestor is valid|


### escrowPayments

*Get escrow payment amount for a subject and schema*


```solidity
function escrowPayments(address subject, bytes32 easUID) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subject`|`address`|The subject address|
|`easUID`|`bytes32`|The EAS schema UID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 The escrowed amount|


### nonces

*Get nonce for an address*


```solidity
function nonces(address addr) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`addr`|`address`|The address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 The current nonce|


### authorizedDelegators

*Check if an address is an authorized delegator*


```solidity
function authorizedDelegators(address delegator) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`delegator`|`address`|The delegator address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if authorized|


### eas

*Get the EAS contract address*


```solidity
function eas() external view returns (IEAS);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`IEAS`|IEAS The EAS contract|


### _decodeAttestationData

*Decode attestation data based on schema field types*


```solidity
function _decodeAttestationData(bytes32 easUID, bytes memory data) internal view returns (bytes[] memory values);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`easUID`|`bytes32`|The EAS schema UID|
|`data`|`bytes`|The raw attestation data|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`values`|`bytes[]`|Array of decoded values|


