# ComplianceFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Offerings/offering/facets/ComplianceFacet.sol)

Handles attestation registry management and compliance checks
Used by all offering types to avoid code duplication

*Centralized compliance management for offering diamonds*


## Functions
### setAttestationRegistry

*Set the attestation registry*


```solidity
function setAttestationRegistry(address attestationRegistry) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`attestationRegistry`|`address`|The address of the attestation registry|


### getAttestationRegistry

*Get the current attestation registry*


```solidity
function getAttestationRegistry() external view returns (address registry);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`registry`|`address`|Address of the current attestation registry|


### _updateSchemaIds

*Update the cached schema IDs from the registry*


```solidity
function _updateSchemaIds(OfferingStorage.Layout storage s) internal;
```

### updateSchemaIds

*Force update schema IDs (useful when schemas are added to registry)*


```solidity
function updateSchemaIds() external;
```

### checkBasicCompliance

*Check if an investor meets basic compliance requirements*


```solidity
function checkBasicCompliance(address investor) external view returns (bool compliant);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`compliant`|`bool`|Whether the investor is compliant|


### checkForeignInvestorStatus

*Check if an investor is a foreign (non-US) person*


```solidity
function checkForeignInvestorStatus(address investor) external view returns (bool isForeign);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`isForeign`|`bool`|Whether the investor is a foreign person|


### checkCustomerIdentification

*Check if an investor is properly identified*


```solidity
function checkCustomerIdentification(address investor) external view returns (bool identified);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`identified`|`bool`|Whether the investor is identified|


### checkBusinessInformation

*Check business information compliance*


```solidity
function checkBusinessInformation(address investor) external view returns (bool compliant);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`compliant`|`bool`|Whether business information is compliant|


### checkRegSCompliance

*Comprehensive compliance check for Regulation S*


```solidity
function checkRegSCompliance(address investor) external view returns (bool compliant);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`compliant`|`bool`|Whether the investor meets Reg S requirements|


### checkRegDCompliance

*Comprehensive compliance check for Regulation D*


```solidity
function checkRegDCompliance(address investor) external view returns (bool compliant);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`compliant`|`bool`|Whether the investor meets Reg D requirements|


### getSchemaIds

*Get all schema IDs*


```solidity
function getSchemaIds()
    external
    view
    returns (bytes32 basicCompliance, bytes32 foreignInvestor, bytes32 customerIdentification, bytes32 basicBusiness);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`basicCompliance`|`bytes32`|Basic compliance schema ID|
|`foreignInvestor`|`bytes32`|Foreign investor schema ID|
|`customerIdentification`|`bytes32`|Customer identification schema ID|
|`basicBusiness`|`bytes32`|Basic business information schema ID|


### isAttestationValidForEntity

*Check if attestation is valid for the offering entity*


```solidity
function isAttestationValidForEntity(address investor, bytes32 schemaUID) external view returns (bool valid);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`schemaUID`|`bytes32`|The schema UID to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`valid`|`bool`|Whether the attestation is valid|


### _requireEntity

*Internal function to check if caller is the entity*


```solidity
function _requireEntity(OfferingStorage.Layout storage s) internal view;
```

## Events
### AttestationRegistryUpdated

```solidity
event AttestationRegistryUpdated(address indexed previousRegistry, address indexed newRegistry);
```

### SchemaIdsUpdated

```solidity
event SchemaIdsUpdated(
    bytes32 basicComplianceSchemaUID,
    bytes32 foreignInvestorSchemaUID,
    bytes32 customerIdentificationSchemaUID,
    bytes32 basicBusinessSchemaUID
);
```

