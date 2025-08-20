# QueryFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Ledgers/ledger/facets/QueryFacet.sol)

Facet for querying account balances and information in the ledger

*Handles balance calculations, account queries, and data retrieval*


## Functions
### onlyInitialized


```solidity
modifier onlyInitialized();
```

### getAccountBalance

Gets the direct balance of an account (excluding children)


```solidity
function getAccountBalance(string memory code) external view onlyInitialized returns (int256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`code`|`string`|The account code|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`int256`|The current balance|


### getAccountTotalBalance

Gets the total balance of an account including all its children recursively


```solidity
function getAccountTotalBalance(string memory code) external view onlyInitialized returns (int256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`code`|`string`|The account code|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`int256`|The total balance including children|


### _getAccountTotalBalanceRecursive

Internal recursive function to calculate total balance including children


```solidity
function _getAccountTotalBalanceRecursive(string memory code) internal view returns (int256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`code`|`string`|The account code|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`int256`|The total balance including children|


### getChildAccounts

Gets all direct child accounts of a parent account


```solidity
function getChildAccounts(string memory parentCode) external view onlyInitialized returns (string[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`parentCode`|`string`|The parent account code|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string[]`|Array of child account codes|


### isDebitNormalForAccount

Determines if an account is debit-normal, considering contra accounts


```solidity
function isDebitNormalForAccount(string memory code) external view onlyInitialized returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`code`|`string`|The account code|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if debit increases the account balance, false otherwise|


### accounts

Get account information


```solidity
function accounts(string memory code)
    external
    view
    onlyInitialized
    returns (string memory, string memory, string memory, int256, uint256, string memory, bool, bool, bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`code`|`string`|The account code|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|code The account code|
|`<none>`|`string`|name The account name|
|`<none>`|`string`|description The account description|
|`<none>`|`int256`|balance The account balance|
|`<none>`|`uint256`|sectionId The section ID|
|`<none>`|`string`|parentCode The parent code|
|`<none>`|`bool`|active Whether the account is active|
|`<none>`|`bool`|isParent Whether the account is a parent|
|`<none>`|`bool`|isContraAccount Whether the account is a contra account|


### accountSections

Get account section information


```solidity
function accountSections(uint256 sectionId)
    external
    view
    onlyInitialized
    returns (LedgerStorage.AccountSectionInfo memory section);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`sectionId`|`uint256`|The section ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`section`|`LedgerStorage.AccountSectionInfo`|The account section struct|


### nextSectionId

Get the next section ID


```solidity
function nextSectionId() external view onlyInitialized returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The next section ID|


### decimals

Get the number of decimal places


```solidity
function decimals() external view onlyInitialized returns (uint8);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|The decimal places|


### currencyCode

Get the currency code


```solidity
function currencyCode() external view onlyInitialized returns (string memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|The ISO currency code|


### accountExists

Check if an account exists


```solidity
function accountExists(string memory code) external view onlyInitialized returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`code`|`string`|The account code|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the account exists|


### isAccountActive

Check if an account is active


```solidity
function isAccountActive(string memory code) external view onlyInitialized returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`code`|`string`|The account code|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the account is active|


### isParentAccount

Check if an account is a parent account


```solidity
function isParentAccount(string memory code) external view onlyInitialized returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`code`|`string`|The account code|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the account is a parent account|


### isContraAccount

Check if an account is a contra account


```solidity
function isContraAccount(string memory code) external view onlyInitialized returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`code`|`string`|The account code|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the account is a contra account|


### getAccountSectionId

Get the section ID for an account


```solidity
function getAccountSectionId(string memory code) external view onlyInitialized returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`code`|`string`|The account code|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The section ID|


### getAccountParentCode

Get the parent code for an account


```solidity
function getAccountParentCode(string memory code) external view onlyInitialized returns (string memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`code`|`string`|The account code|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|The parent account code (empty string if top-level)|


### sectionExists

Check if a section exists


```solidity
function sectionExists(uint256 sectionId) external view onlyInitialized returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`sectionId`|`uint256`|The section ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the section exists|


### isSectionActive

Check if a section is active


```solidity
function isSectionActive(uint256 sectionId) external view onlyInitialized returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`sectionId`|`uint256`|The section ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the section is active|


### getSectionCodeRange

Get the code range for a section


```solidity
function getSectionCodeRange(uint256 sectionId)
    external
    view
    onlyInitialized
    returns (uint256 codeStart, uint256 codeEnd);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`sectionId`|`uint256`|The section ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`codeStart`|`uint256`|The start of the code range|
|`codeEnd`|`uint256`|The end of the code range|


### getSectionName

Get the section name


```solidity
function getSectionName(uint256 sectionId) external view onlyInitialized returns (string memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`sectionId`|`uint256`|The section ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|The section name|


