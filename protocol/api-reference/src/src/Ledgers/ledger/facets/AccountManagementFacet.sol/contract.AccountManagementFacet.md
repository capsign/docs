# AccountManagementFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Ledgers/ledger/facets/AccountManagementFacet.sol)

Facet for managing accounts and account sections in the ledger

*Handles account creation, updates, sections, and parent-child relationships*


## Functions
### onlyOwner


```solidity
modifier onlyOwner();
```

### onlyInitialized


```solidity
modifier onlyInitialized();
```

### createAccountSection

Create a new account section


```solidity
function createAccountSection(string memory name, uint256 codeStart, uint256 codeEnd, bool debitIncreases)
    external
    onlyOwner
    onlyInitialized
    returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|Section name (e.g., "Assets", "Liabilities")|
|`codeStart`|`uint256`|Start of the code range|
|`codeEnd`|`uint256`|End of the code range|
|`debitIncreases`|`bool`|Whether debits increase account balances in this section|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The ID of the newly created section|


### _createAccountSection

Internal function to create a new account section without access control


```solidity
function _createAccountSection(string memory name, uint256 codeStart, uint256 codeEnd, bool debitIncreases)
    internal
    returns (uint256);
```

### updateAccountSection

Update an existing account section


```solidity
function updateAccountSection(uint256 sectionId, string memory name, bool active) external onlyOwner onlyInitialized;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`sectionId`|`uint256`|The ID of the section to update|
|`name`|`string`|The new name for the section|
|`active`|`bool`|Whether the section should be active|


### getAccountSection

Get information about an account section


```solidity
function getAccountSection(uint256 sectionId)
    external
    view
    onlyInitialized
    returns (string memory name, uint256 codeStart, uint256 codeEnd, bool sectionIsDebitNormal, bool active);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`sectionId`|`uint256`|The section ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|The section name|
|`codeStart`|`uint256`|The start of code range|
|`codeEnd`|`uint256`|The end of code range|
|`sectionIsDebitNormal`|`bool`|Whether debits increase account balances|
|`active`|`bool`|Whether the section is active|


### isDebitNormal

Determines if an account is debit-normal based on its section


```solidity
function isDebitNormal(uint256 sectionId) external view onlyInitialized returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`sectionId`|`uint256`|The section ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if debit increases the account balance, false otherwise|


### createStandardSections

Create standard account sections


```solidity
function createStandardSections() external onlyOwner onlyInitialized;
```

### createAccount

Create a new account in the ledger using dot notation


```solidity
function createAccount(
    string memory code,
    string memory name,
    string memory description,
    uint256 sectionId,
    string memory parentCode,
    bool isParent,
    bool isContraAccount
) external onlyOwner onlyInitialized;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`code`|`string`|The account code using dot notation (e.g., "1000.100.10")|
|`name`|`string`|The human-readable name of the account|
|`description`|`string`|The detailed description of the account's purpose|
|`sectionId`|`uint256`|The section this account belongs to|
|`parentCode`|`string`|The parent account code (empty string if top-level)|
|`isParent`|`bool`|Whether this account can have child accounts|
|`isContraAccount`|`bool`|Whether this is a contra account (inverts normal debit/credit behavior)|


### updateAccount

Update an existing account's details


```solidity
function updateAccount(
    string memory code,
    string memory name,
    string memory description,
    bool active,
    bool isContraAccount
) external onlyOwner onlyInitialized;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`code`|`string`|The account code to update|
|`name`|`string`|The new name for the account|
|`description`|`string`|The new description for the account|
|`active`|`bool`|Whether the account should be active or inactive|
|`isContraAccount`|`bool`|Whether this is a contra account (inverts normal debit/credit behavior)|


### toggleContraAccountStatus

Toggle the contra account status


```solidity
function toggleContraAccountStatus(string memory code) external onlyOwner onlyInitialized;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`code`|`string`|The account code|


### setAccountParentStatus

Set account parent status


```solidity
function setAccountParentStatus(string memory code, bool isParent) external onlyOwner onlyInitialized;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`code`|`string`|The account code|
|`isParent`|`bool`|Whether the account should be a parent|


### updateAccountParent

Update the parent of an existing account


```solidity
function updateAccountParent(string memory accountCode, string memory newParentCode)
    external
    onlyOwner
    onlyInitialized
    returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`accountCode`|`string`|The account code to update|
|`newParentCode`|`string`|The new parent account code (empty string for top-level)|


### reparentAccount

Reparent an account by creating a new account with proper code format and transferring all data


```solidity
function reparentAccount(string memory oldCode, string memory newParentCode, string memory newSuffix)
    external
    onlyOwner
    onlyInitialized
    returns (string memory newCode);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oldCode`|`string`|The current account code|
|`newParentCode`|`string`|The new parent account code (empty string for top-level)|
|`newSuffix`|`string`|The new suffix to append to the parent code (e.g., "20" for "1000.20")|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`newCode`|`string`|The new account code|


### validateAccountCode

Validates that an account code follows dot notation format and belongs to the correct section


```solidity
function validateAccountCode(string memory code, uint256 sectionId) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`code`|`string`|The account code to validate|
|`sectionId`|`uint256`|The section this account should belong to|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|rootNumber The root number of the account code|


### isDirectChild

Check if a code is a direct child of a parent code


```solidity
function isDirectChild(string memory parentCode, string memory childCode) internal pure returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`parentCode`|`string`|The potential parent code|
|`childCode`|`string`|The potential child code|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if childCode is a direct child of parentCode|


### uint2str

Helper function to convert uint to string


```solidity
function uint2str(uint256 _i) internal pure returns (string memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_i`|`uint256`|The uint to convert|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|The string representation|


### removeChildAccount

*Helper function to remove a child from a parent's children array*


```solidity
function removeChildAccount(string memory parentCode, string memory childCode) internal;
```

### isChildAccount

*Helper function to check if an account is a child (direct or indirect) of another account*


```solidity
function isChildAccount(string memory potentialParentCode, string memory checkAccountCode)
    internal
    view
    returns (bool);
```

## Events
### AccountSectionCreated

```solidity
event AccountSectionCreated(uint256 id, string name, uint256 codeStart, uint256 codeEnd, bool isDebitNormal);
```

### AccountSectionUpdated

```solidity
event AccountSectionUpdated(uint256 id, string name, bool active);
```

### AccountCreated

```solidity
event AccountCreated(
    string code,
    string name,
    string description,
    uint256 sectionId,
    string parentCode,
    bool isParent,
    bool isContraAccount
);
```

### AccountUpdated

```solidity
event AccountUpdated(string code, string name, string description, bool active, bool isContraAccount);
```

### AccountContraStatusUpdated

```solidity
event AccountContraStatusUpdated(string code, bool isContraAccount);
```

### AccountParentUpdated

```solidity
event AccountParentUpdated(string accountCode, string oldParentCode, string newParentCode);
```

### AccountParentStatusUpdated

```solidity
event AccountParentStatusUpdated(string code, bool isParent);
```

### AccountReparented

```solidity
event AccountReparented(string oldCode, string newCode, string oldParentCode, string newParentCode);
```

