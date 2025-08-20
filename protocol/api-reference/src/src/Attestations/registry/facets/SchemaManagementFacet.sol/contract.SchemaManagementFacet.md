# SchemaManagementFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Attestations/registry/facets/SchemaManagementFacet.sol)

**Inherits:**
[AccessManaged](/src/Authorization/AccessManaged.sol/abstract.AccessManaged.md)

*Facet for managing attestation schemas in the AttestationRegistry diamond*


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


### registerSchema

*Register a new schema in the registry*


```solidity
function registerSchema(
    string calldata name,
    bytes32 easUID,
    bool revocable,
    uint64 expirationPeriod,
    bool isTimeSeries,
    string[] calldata fieldNames,
    string[] calldata fieldTypes
) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|Human-readable name for the schema|
|`easUID`|`bytes32`|The EAS schema UID|
|`revocable`|`bool`|Whether attestations can be revoked|
|`expirationPeriod`|`uint64`|Default expiration period in seconds (0 = no expiration)|
|`isTimeSeries`|`bool`|Whether this schema supports multiple attestations per subject (e.g., valuations over time)|
|`fieldNames`|`string[]`|Names of the fields in the schema|
|`fieldTypes`|`string[]`|ABI types for each field|


### deregisterSchema

*Deregister a schema (mark as inactive)*


```solidity
function deregisterSchema(bytes32 easUID) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`easUID`|`bytes32`|The EAS schema UID to deregister|


### reactivateSchema

*Reactivate a previously deregistered schema*


```solidity
function reactivateSchema(bytes32 easUID) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`easUID`|`bytes32`|The EAS schema UID to reactivate|


### setAttestationPaymentAmount

*Set the payment amount required for attestations of a specific schema*


```solidity
function setAttestationPaymentAmount(bytes32 easUID, uint256 amount) external restricted;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`easUID`|`bytes32`|The EAS schema UID|
|`amount`|`uint256`|The payment amount in wei|


### schemas

*Get a schema by its EAS UID*


```solidity
function schemas(bytes32 easUID) external view returns (IAttestationRegistry.Schema memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`easUID`|`bytes32`|The EAS schema UID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`IAttestationRegistry.Schema`|Schema The schema struct|


### schemaNameToUID

*Get schema UID by name*


```solidity
function schemaNameToUID(string calldata name) external view returns (bytes32);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|The schema name|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|bytes32 The EAS schema UID|


### attestationPaymentAmounts

*Get the payment amount for a schema*


```solidity
function attestationPaymentAmounts(bytes32 easUID) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`easUID`|`bytes32`|The EAS schema UID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 The payment amount in wei|


## Events
### SchemaRegistered

```solidity
event SchemaRegistered(bytes32 indexed easUID, string name, bool isTimeSeries);
```

### SchemaUpdated

```solidity
event SchemaUpdated(bytes32 indexed easUID, string name, bool active);
```

### AttestationPaymentAmountSet

```solidity
event AttestationPaymentAmountSet(bytes32 indexed easUID, uint256 amount);
```

