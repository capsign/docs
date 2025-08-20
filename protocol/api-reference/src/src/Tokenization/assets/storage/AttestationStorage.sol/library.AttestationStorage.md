# AttestationStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/storage/AttestationStorage.sol)

Storage library for EAS attestation-based compliance functionality

*Manages storage for EAS attestation-based compliance requirements*


## State Variables
### ATTESTATION_STORAGE_POSITION

```solidity
bytes32 constant ATTESTATION_STORAGE_POSITION = keccak256("capsign.storage.attestations");
```


## Functions
### layout


```solidity
function layout() internal pure returns (Layout storage l);
```

### setAttestationRegistry


```solidity
function setAttestationRegistry(address registry) internal;
```

### getAttestationRegistry


```solidity
function getAttestationRegistry() internal view returns (address);
```

### setLotComplianceRequirement


```solidity
function setLotComplianceRequirement(uint256 customLotId, ComplianceRequirement memory requirement) internal;
```

### getLotComplianceRequirement


```solidity
function getLotComplianceRequirement(uint256 customLotId) internal view returns (ComplianceRequirement memory);
```

### clearLotComplianceRequirement


```solidity
function clearLotComplianceRequirement(uint256 customLotId) internal;
```

### hasLotComplianceRequirement


```solidity
function hasLotComplianceRequirement(uint256 customLotId) internal view returns (bool);
```

### setCachedComplianceLevel


```solidity
function setCachedComplianceLevel(address investor, uint8 level) internal;
```

### getCachedComplianceLevel


```solidity
function getCachedComplianceLevel(address investor) internal view returns (uint8 level, bool isValid);
```

### setComplianceCacheDuration


```solidity
function setComplianceCacheDuration(uint256 duration) internal;
```

### getComplianceCacheDuration


```solidity
function getComplianceCacheDuration() internal view returns (uint256);
```

### setDefaultComplianceRequirement


```solidity
function setDefaultComplianceRequirement(ComplianceRequirement memory requirement) internal;
```

### clearDefaultComplianceRequirement


```solidity
function clearDefaultComplianceRequirement() internal;
```

### getDefaultComplianceRequirement


```solidity
function getDefaultComplianceRequirement() internal view returns (ComplianceRequirement memory);
```

### hasDefaultRequirement


```solidity
function hasDefaultRequirement() internal view returns (bool);
```

### getComplianceLevelString

Get compliance level as string for display


```solidity
function getComplianceLevelString(uint8 level) internal pure returns (string memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`level`|`uint8`|The compliance level|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|String representation of compliance level|


### getBusinessTypeString

Get business type as string for display


```solidity
function getBusinessTypeString(uint8 businessType) internal pure returns (string memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`businessType`|`uint8`|The business type|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|String representation of business type|


### isBusinessTypeAllowed

Check if business type is allowed


```solidity
function isBusinessTypeAllowed(uint8 businessType, uint8[] memory allowedTypes) internal pure returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`businessType`|`uint8`|The business type to check|
|`allowedTypes`|`uint8[]`|Array of allowed business types|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if business type is allowed|


### isNaicsCodeBlocked

Check if NAICS code is blocked


```solidity
function isNaicsCodeBlocked(uint32 naicsCode, uint32[] memory blockedCodes) internal pure returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`naicsCode`|`uint32`|The NAICS code to check|
|`blockedCodes`|`uint32[]`|Array of blocked NAICS codes|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if NAICS code is blocked|


### getRequirementSummary

Get compliance requirement summary for display


```solidity
function getRequirementSummary(ComplianceRequirement memory requirement)
    internal
    pure
    returns (string memory summary);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requirement`|`ComplianceRequirement`|The compliance requirement|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`summary`|`string`|String summary of the requirement|


### validateComplianceRequirement

Validate compliance requirement parameters


```solidity
function validateComplianceRequirement(ComplianceRequirement memory requirement)
    internal
    pure
    returns (bool isValid, string memory errorMessage);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requirement`|`ComplianceRequirement`|The compliance requirement to validate|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`isValid`|`bool`|True if requirement is valid|
|`errorMessage`|`string`|Error message if invalid|


### getAllLotsWithRequirements

Get all lots with compliance requirements


```solidity
function getAllLotsWithRequirements(uint256 maxLots)
    internal
    view
    returns (uint256[] memory lotIds, ComplianceRequirement[] memory requirements);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`maxLots`|`uint256`|Maximum number of lots to check (gas limit protection)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`lotIds`|`uint256[]`|Array of lot IDs with requirements|
|`requirements`|`ComplianceRequirement[]`|Array of corresponding requirements|


### areRequirementsEquivalent

Check if two compliance requirements are equivalent


```solidity
function areRequirementsEquivalent(ComplianceRequirement memory req1, ComplianceRequirement memory req2)
    internal
    pure
    returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`req1`|`ComplianceRequirement`|First requirement|
|`req2`|`ComplianceRequirement`|Second requirement|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if requirements are equivalent|


## Structs
### ComplianceRequirement

```solidity
struct ComplianceRequirement {
    uint8 minComplianceLevel;
    string jurisdiction;
    uint8[] allowedBusinessTypes;
    uint32[] blockedNaicsCodes;
    bool requirePrivateReference;
    bool isActive;
}
```

### Layout

```solidity
struct Layout {
    address attestationRegistry;
    mapping(uint256 => ComplianceRequirement) lotComplianceRequirements;
    mapping(address => uint8) investorComplianceLevels;
    mapping(address => uint256) lastComplianceCheck;
    uint256 complianceCacheDuration;
    ComplianceRequirement defaultComplianceRequirement;
    bool hasDefaultComplianceRequirement;
}
```

