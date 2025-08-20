# SanctionsFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/facets/SanctionsFacet.sol)

**Inherits:**
[IComplianceCondition](/src/Tokenization/assets/interfaces/IComplianceCondition.sol/interface.IComplianceCondition.md)

Facet for managing sanctions compliance

*Implements sanctions checking as a compliance condition facet*


## State Variables
### SANCTIONS_STORAGE_POSITION

```solidity
bytes32 constant SANCTIONS_STORAGE_POSITION = keccak256("capsign.storage.Sanctions");
```


## Functions
### sanctionsStorage


```solidity
function sanctionsStorage() internal pure returns (SanctionsStorage storage ss);
```

### setSanctionsEnabled

Enable or disable sanctions checking


```solidity
function setSanctionsEnabled(bool enabled) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`enabled`|`bool`|Whether sanctions checking should be enabled|


### setSanctionsOracle

Set the sanctions oracle address


```solidity
function setSanctionsOracle(address oracle) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oracle`|`address`|The address of the sanctions oracle contract|


### sanctionAddress

Add an address to the sanctions list


```solidity
function sanctionAddress(address account, string memory reason) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address to sanction|
|`reason`|`string`|The reason for sanctioning|


### unsanctionAddress

Remove an address from the sanctions list


```solidity
function unsanctionAddress(address account) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address to unsanction|


### sanctionCountry

Add a country to the sanctions list


```solidity
function sanctionCountry(string memory countryCode) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`countryCode`|`string`|ISO country code (e.g., "US", "CN")|


### unsanctionCountry

Remove a country from the sanctions list


```solidity
function unsanctionCountry(string memory countryCode) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`countryCode`|`string`|ISO country code|


### setAddressCountry

Set the country for an address


```solidity
function setAddressCountry(address account, string memory countryCode) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address|
|`countryCode`|`string`|ISO country code|


### batchSetAddressCountries

Batch set countries for multiple addresses


```solidity
function batchSetAddressCountries(address[] calldata accounts, string[] calldata countryCodes) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`accounts`|`address[]`|Array of addresses|
|`countryCodes`|`string[]`|Array of country codes|


### isAddressSanctioned

Check if an address is sanctioned


```solidity
function isAddressSanctioned(address account) public view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the address is sanctioned|


### isCountrySanctioned

Check if a country is sanctioned


```solidity
function isCountrySanctioned(string memory countryCode) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`countryCode`|`string`|ISO country code|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the country is sanctioned|


### getAddressCountry

Get the country for an address


```solidity
function getAddressCountry(address account) external view returns (string memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|The country code|


### getSanctionedAddresses

Get all sanctioned addresses


```solidity
function getSanctionedAddresses() external view returns (address[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Array of sanctioned addresses|


### getSanctionedCountries

Get all sanctioned countries


```solidity
function getSanctionedCountries() external view returns (string[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string[]`|Array of sanctioned country codes|


### sanctionsEnabled

Check if sanctions are enabled


```solidity
function sanctionsEnabled() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if sanctions checking is enabled|


### getSanctionsOracle

Get the current sanctions oracle address


```solidity
function getSanctionsOracle() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The oracle address|


### _passesBasicCompliance

*Check whether a subject passes Basic Compliance Status for this token's entity
- Requires EAS registry to be configured via AttestationStorage
- Verifies attestation validity for the token entity (issuer of record)
- Ensures isCompliant == true, sanctionsStatus == true, not expired, and non-zero verificationHash*


```solidity
function _passesBasicCompliance(address subject) internal view returns (bool);
```

### validateSanctionsTransfer

Validate if a transfer complies with sanctions rules


```solidity
function validateSanctionsTransfer(address from, address to, uint256, uint256) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The sender address|
|`to`|`address`|The recipient address|
|`<none>`|`uint256`||
|`<none>`|`uint256`||

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if transfer is allowed|


### processSanctionsTransfer

Process a transfer through sanctions rules


```solidity
function processSanctionsTransfer(address from, address to, uint256 customLotId, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The sender address|
|`to`|`address`|The recipient address|
|`customLotId`|`uint256`|The custom lot ID|
|`amount`|`uint256`|The amount being transferred|


### emergencyBlockAddress

Emergency function to block all transfers for an address


```solidity
function emergencyBlockAddress(address account, string memory reason) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address to block|
|`reason`|`string`|The reason for blocking|


### bulkCheckSanctions

Bulk check sanctions status for multiple addresses


```solidity
function bulkCheckSanctions(address[] calldata accounts) external view returns (bool[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`accounts`|`address[]`|Array of addresses to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool[]`|Array of sanctions status (true = sanctioned)|


### validateTransfer

Validate if a transfer complies with sanctions rules (IComplianceCondition interface)


```solidity
function validateTransfer(address from, address to, uint256 lotId, uint256 amount) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The sender address|
|`to`|`address`|The recipient address|
|`lotId`|`uint256`|The lot ID (unused for sanctions)|
|`amount`|`uint256`|The amount being transferred (unused for sanctions)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if transfer is allowed|


### processTransfer

Process a transfer through sanctions rules (IComplianceCondition interface)


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
|`data`|`bytes`|Additional data (unused for sanctions)|


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
### AddressSanctioned

```solidity
event AddressSanctioned(address indexed account, string reason);
```

### AddressUnsanctioned

```solidity
event AddressUnsanctioned(address indexed account);
```

### CountrySanctioned

```solidity
event CountrySanctioned(string indexed countryCode);
```

### CountryUnsanctioned

```solidity
event CountryUnsanctioned(string indexed countryCode);
```

### SanctionsOracleUpdated

```solidity
event SanctionsOracleUpdated(address indexed oldOracle, address indexed newOracle);
```

### SanctionsStatusChanged

```solidity
event SanctionsStatusChanged(bool enabled);
```

### TransferBlocked

```solidity
event TransferBlocked(address indexed from, address indexed to, uint256 amount, string reason);
```

## Errors
### Sanctions_AddressSanctioned

```solidity
error Sanctions_AddressSanctioned(address account);
```

### Sanctions_CountrySanctioned

```solidity
error Sanctions_CountrySanctioned(string country);
```

### Sanctions_TransferBlocked

```solidity
error Sanctions_TransferBlocked();
```

### Sanctions_InvalidCountryCode

```solidity
error Sanctions_InvalidCountryCode();
```

## Structs
### SanctionsStorage

```solidity
struct SanctionsStorage {
    mapping(address => bool) sanctionedAddresses;
    mapping(string => bool) sanctionedCountries;
    mapping(address => string) addressCountries;
    address[] sanctionedList;
    string[] sanctionedCountryList;
    bool sanctionsEnabled;
    address sanctionsOracle;
}
```

