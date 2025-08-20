# Rule144Facet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/facets/Rule144Facet.sol)

Rule 144 provides a safe harbor for the resale of restricted or control securities
if certain conditions are met: holding periods, volume limits, current public info, manner of sale

*Asset facet implementing SEC Rule 144 requirements for resale of restricted securities*


## State Variables
### REPORTING_COMPANY_HOLDING_PERIOD

```solidity
uint256 public constant REPORTING_COMPANY_HOLDING_PERIOD = 180 days;
```


### NON_REPORTING_COMPANY_HOLDING_PERIOD

```solidity
uint256 public constant NON_REPORTING_COMPANY_HOLDING_PERIOD = 365 days;
```


### VOLUME_LIMIT_PERIOD

```solidity
uint256 public constant VOLUME_LIMIT_PERIOD = 28 days;
```


### VOLUME_LIMIT_PERCENTAGE

```solidity
uint256 public constant VOLUME_LIMIT_PERCENTAGE = 100;
```


### RULE144_STORAGE_SLOT

```solidity
bytes32 private constant RULE144_STORAGE_SLOT = keccak256("capsign.storage.Rule144");
```


## Functions
### _getRule144Storage

*Get Rule 144 storage*


```solidity
function _getRule144Storage() internal pure returns (Rule144Storage storage s);
```

### initializeRule144

*Initialize Rule 144 functionality*


```solidity
function initializeRule144(bool _isReporting, bool _hasPublicInfo) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_isReporting`|`bool`|Whether the company is subject to Exchange Act reporting|
|`_hasPublicInfo`|`bool`|Whether current public information is available|


### setCompanyStatus

*Set company reporting status and public information availability*


```solidity
function setCompanyStatus(bool _isReporting, bool _hasPublicInfo) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_isReporting`|`bool`|Whether the company is subject to Exchange Act reporting|
|`_hasPublicInfo`|`bool`|Whether current public information is available|


### setControlPerson

*Designate control persons (affiliates) for volume limit purposes*


```solidity
function setControlPerson(address person, bool _isControl) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`person`|`address`|The person to designate|
|`_isControl`|`bool`|Whether the person is a control person|


### setRule144Enabled

*Enable or disable Rule 144 enforcement*


```solidity
function setRule144Enabled(bool enabled) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`enabled`|`bool`|Whether Rule 144 should be enforced|


### checkRule144Transfer

*Check if a transfer is allowed under Rule 144*


```solidity
function checkRule144Transfer(address from, address to, bytes32 lotId, uint256 amount)
    external
    view
    returns (bool allowed, string memory reason);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The seller|
|`to`|`address`|The buyer|
|`lotId`|`bytes32`|The lot being transferred|
|`amount`|`uint256`|The amount being transferred|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`allowed`|`bool`|Whether the transfer is allowed|
|`reason`|`string`|Reason if transfer is not allowed|


### _checkVolumeLimit

*Check if volume limit is satisfied for control person*


```solidity
function _checkVolumeLimit(Rule144Storage storage s, address seller, uint256 amount) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`s`|`Rule144Storage`|Rule 144 storage|
|`seller`|`address`|The control person selling|
|`amount`|`uint256`|The amount being sold|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|Whether volume limit is satisfied|


### recordRule144Sale

*Record a sale for volume tracking (called after successful transfer)*


```solidity
function recordRule144Sale(address from, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The seller|
|`amount`|`uint256`|The amount sold|


### getVolumeInfo

*Get volume information for a control person*


```solidity
function getVolumeInfo(address person)
    external
    view
    returns (uint256 volumeSold, uint256 volumeLimit, uint256 lastSale);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`person`|`address`|The control person|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`volumeSold`|`uint256`|Volume sold in current 4-week period|
|`volumeLimit`|`uint256`|Maximum volume allowed (1% of outstanding)|
|`lastSale`|`uint256`|Timestamp of last sale|


### getHoldingPeriodInfo

*Get holding period information for a lot*


```solidity
function getHoldingPeriodInfo(bytes32 lotId)
    external
    view
    returns (uint256 acquisitionDate, uint256 requiredHoldingPeriod, bool holdingPeriodMet);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotId`|`bytes32`|The lot ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`acquisitionDate`|`uint256`|When the lot was acquired|
|`requiredHoldingPeriod`|`uint256`|Required holding period based on company status|
|`holdingPeriodMet`|`bool`|Whether holding period is satisfied|


### getCompanyStatus

*Get company status information*


```solidity
function getCompanyStatus() external view returns (bool isReporting, bool hasPublicInfo, bool enabled);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`isReporting`|`bool`|Whether company is subject to Exchange Act reporting|
|`hasPublicInfo`|`bool`|Whether current public information is available|
|`enabled`|`bool`|Whether Rule 144 enforcement is enabled|


### isControlPerson

*Check if an address is designated as a control person*


```solidity
function isControlPerson(address person) external view returns (bool isControl);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`person`|`address`|The address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`isControl`|`bool`|Whether the person is a control person|


### getControlPersons

*Get list of all control persons (note: this is expensive, use carefully)*


```solidity
function getControlPersons() external pure returns (address[] memory controlPersons);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`controlPersons`|`address[]`|Array of control person addresses|


## Events
### CompanyStatusUpdated

```solidity
event CompanyStatusUpdated(bool isReporting, bool hasPublicInfo);
```

### ControlPersonDesignated

```solidity
event ControlPersonDesignated(address indexed person, bool isControl);
```

### Rule144SaleRecorded

```solidity
event Rule144SaleRecorded(address indexed seller, uint256 amount, uint256 timestamp);
```

### Rule144EnabledUpdated

```solidity
event Rule144EnabledUpdated(bool enabled);
```

## Structs
### Rule144Storage

```solidity
struct Rule144Storage {
    bool isReportingCompany;
    bool hasCurrentPublicInformation;
    mapping(address => bool) isControlPerson;
    mapping(address => uint256) volumeSoldInPeriod;
    mapping(address => uint256) lastSaleTimestamp;
    bool rule144Enabled;
}
```

