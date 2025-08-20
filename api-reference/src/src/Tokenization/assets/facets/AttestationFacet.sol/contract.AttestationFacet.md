# AttestationFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/facets/AttestationFacet.sol)

Facet for EAS attestation-based compliance

*Implements compliance logic using EAS attestations for investor qualification*


## State Variables
### COMPLIANCE_LEVEL_BASIC

```solidity
uint8 public constant COMPLIANCE_LEVEL_BASIC = 0;
```


### COMPLIANCE_LEVEL_ENHANCED

```solidity
uint8 public constant COMPLIANCE_LEVEL_ENHANCED = 1;
```


### COMPLIANCE_LEVEL_INSTITUTIONAL

```solidity
uint8 public constant COMPLIANCE_LEVEL_INSTITUTIONAL = 2;
```


### BUSINESS_TYPE_INDIVIDUAL

```solidity
uint8 public constant BUSINESS_TYPE_INDIVIDUAL = 0;
```


### BUSINESS_TYPE_CORPORATION

```solidity
uint8 public constant BUSINESS_TYPE_CORPORATION = 1;
```


### BUSINESS_TYPE_PARTNERSHIP

```solidity
uint8 public constant BUSINESS_TYPE_PARTNERSHIP = 2;
```


### BUSINESS_TYPE_TRUST

```solidity
uint8 public constant BUSINESS_TYPE_TRUST = 3;
```


## Functions
### setAttestationRegistry

Set the attestation registry address


```solidity
function setAttestationRegistry(address registry) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`registry`|`address`|Address of the EAS attestation registry|


### setDefaultComplianceRequirement

Set a default token-level compliance requirement applied to newly created lots

*This does not retroactively change existing lots. Use per-lot setters to override.*


```solidity
function setDefaultComplianceRequirement(
    uint8 minComplianceLevel,
    string memory jurisdiction,
    uint8[] memory allowedBusinessTypes,
    uint32[] memory blockedNaicsCodes,
    bool requirePrivateReference
) external;
```

### clearDefaultComplianceRequirement

Clear the default token-level compliance requirement


```solidity
function clearDefaultComplianceRequirement() external;
```

### setLotComplianceRequirement

Set compliance requirements for a specific lot


```solidity
function setLotComplianceRequirement(
    uint256 customLotId,
    uint8 minComplianceLevel,
    string memory jurisdiction,
    uint8[] memory allowedBusinessTypes,
    uint32[] memory blockedNaicsCodes,
    bool requirePrivateReference
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`customLotId`|`uint256`|The custom lot ID|
|`minComplianceLevel`|`uint8`|Minimum required compliance level (0=basic, 1=enhanced, 2=institutional)|
|`jurisdiction`|`string`|Required jurisdiction for private reference (empty for any)|
|`allowedBusinessTypes`|`uint8[]`|Array of allowed business types (empty = all allowed)|
|`blockedNaicsCodes`|`uint32[]`|Array of blocked NAICS codes (empty = none blocked)|
|`requirePrivateReference`|`bool`|Whether private compliance reference is required|


### checkBasicCompliance

Check if an investor has valid basic compliance status


```solidity
function checkBasicCompliance(address investor, uint256 customLotId) external view returns (bool isCompliant);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`customLotId`|`uint256`|The custom lot ID (to get requirements)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`isCompliant`|`bool`|Whether the investor passes compliance checks|


### checkBasicBusinessInformation

Check if an investor has valid business information


```solidity
function checkBasicBusinessInformation(address investor, uint256 customLotId) external view returns (bool isValid);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`customLotId`|`uint256`|The custom lot ID (to get requirements)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`isValid`|`bool`|Whether the investor passes business checks|


### checkPrivateComplianceReference

Check if an investor has enhanced private compliance reference


```solidity
function checkPrivateComplianceReference(address investor, uint256 customLotId)
    external
    view
    returns (bool hasPrivateReference);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`customLotId`|`uint256`|The custom lot ID (to get requirements)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`hasPrivateReference`|`bool`|Whether investor has valid private compliance reference|


### isAccredited

Helper to check if investor is accredited (enhanced compliance level)


```solidity
function isAccredited(address investor) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|isAccredited Whether investor has enhanced compliance level or higher|


### isInstitutional

Helper to check if investor is institutional (institutional compliance level)


```solidity
function isInstitutional(address investor) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|isInstitutional Whether investor has institutional compliance level|


### getComplianceLevel

Get investor's compliance level


```solidity
function getComplianceLevel(address investor) external view returns (uint8 complianceLevel);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`complianceLevel`|`uint8`|The investor's compliance level (0=basic, 1=enhanced, 2=institutional)|


### getLotComplianceRequirement

Get compliance requirements for a lot


```solidity
function getLotComplianceRequirement(uint256 customLotId)
    external
    view
    returns (AttestationStorage.ComplianceRequirement memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`customLotId`|`uint256`|The custom lot ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`AttestationStorage.ComplianceRequirement`|The compliance requirement|


### getAttestationRegistry

Get the current attestation registry address


```solidity
function getAttestationRegistry() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|Address of the attestation registry|


### _checkInvestorCompliance

Internal function to check investor compliance


```solidity
function _checkInvestorCompliance(address investor, uint256 customLotId) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`customLotId`|`uint256`|The custom lot ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if investor is compliant|


### _getComplianceFailureReason

Get compliance failure reason


```solidity
function _getComplianceFailureReason(address from, address to, uint256 customLotId)
    internal
    view
    returns (string memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|Sender address|
|`to`|`address`|Recipient address|
|`customLotId`|`uint256`|The custom lot ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|Reason string|


### _checkBasicCompliance

*Check if an investor has valid basic compliance status*


```solidity
function _checkBasicCompliance(
    IAttestationRegistry registry,
    address investor,
    address entity,
    uint8 minComplianceLevel
) internal view returns (bool isCompliant);
```

### _checkBasicBusinessInformation

*Check if an investor has valid business information*


```solidity
function _checkBasicBusinessInformation(
    IAttestationRegistry registry,
    address investor,
    address entity,
    uint8[] memory allowedBusinessTypes,
    uint32[] memory blockedNaicsCodes
) internal view returns (bool isValid);
```

### _checkPrivateComplianceReference

*Check if an investor has enhanced private compliance reference*


```solidity
function _checkPrivateComplianceReference(
    IAttestationRegistry registry,
    address investor,
    address entity,
    string memory jurisdiction
) internal view returns (bool hasPrivateReference);
```

### _getComplianceLevel

*Get investor's compliance level*


```solidity
function _getComplianceLevel(IAttestationRegistry registry, address investor, address entity)
    internal
    view
    returns (uint8 complianceLevel);
```

### validateTransfer

Validate if a transfer complies with attestation requirements (IComplianceCondition interface)


```solidity
function validateTransfer(address from, address to, uint256 lotId, uint256 amount) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The sender address|
|`to`|`address`|The recipient address|
|`lotId`|`uint256`|The lot ID|
|`amount`|`uint256`|The amount being transferred|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if transfer is allowed|


### processTransfer

Process a transfer through attestation compliance rules (IComplianceCondition interface)


```solidity
function processTransfer(address from, address to, uint256 lotId, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The sender address|
|`to`|`address`|The recipient address|
|`lotId`|`uint256`|The lot ID|
|`amount`|`uint256`|The amount being transferred|


### processIssuance

Process issuance (IComplianceCondition interface)


```solidity
function processIssuance(address to, uint256 lotId, uint256 amount, bytes calldata data) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|The recipient address|
|`lotId`|`uint256`|The lot ID|
|`amount`|`uint256`|The amount being issued|
|`data`|`bytes`|Additional data (unused for attestation compliance)|


### processRedemption

Process redemption (IComplianceCondition interface)


```solidity
function processRedemption(address from, uint256 lotId, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The sender address|
|`lotId`|`uint256`|The lot ID|
|`amount`|`uint256`|The amount being redeemed|


### migrateLotState

Migrate lot state (IComplianceCondition interface)


```solidity
function migrateLotState(bytes32 oldLotId, bytes32 newLotId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oldLotId`|`bytes32`|The old lot ID|
|`newLotId`|`bytes32`|The new lot ID|


### conditionName

Get the condition name (IComplianceCondition interface)


```solidity
function conditionName() external pure returns (string memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|The name of this condition|


## Events
### AttestationRegistrySet

```solidity
event AttestationRegistrySet(address indexed registry);
```

### LotComplianceRequirementSet

```solidity
event LotComplianceRequirementSet(uint256 indexed customLotId, uint8 minComplianceLevel, bool requirePrivateReference);
```

### ComplianceCheckPassed

```solidity
event ComplianceCheckPassed(address indexed investor, uint256 indexed customLotId, uint8 complianceLevel);
```

### ComplianceCheckFailed

```solidity
event ComplianceCheckFailed(address indexed investor, uint256 indexed customLotId, string reason);
```

### TransferBlocked

```solidity
event TransferBlocked(address indexed from, address indexed to, uint256 amount, string reason);
```

### DefaultComplianceRequirementSet

```solidity
event DefaultComplianceRequirementSet(uint8 minComplianceLevel, string jurisdiction, bool requirePrivateReference);
```

## Errors
### Attestation_RegistryNotSet

```solidity
error Attestation_RegistryNotSet();
```

### Attestation_BasicComplianceFailed

```solidity
error Attestation_BasicComplianceFailed();
```

### Attestation_BusinessInformationFailed

```solidity
error Attestation_BusinessInformationFailed();
```

### Attestation_PrivateReferenceFailed

```solidity
error Attestation_PrivateReferenceFailed();
```

### Attestation_TransferNotAllowed

```solidity
error Attestation_TransferNotAllowed();
```

### Attestation_InvalidComplianceLevel

```solidity
error Attestation_InvalidComplianceLevel();
```

### Attestation_InvalidBusinessType

```solidity
error Attestation_InvalidBusinessType();
```

