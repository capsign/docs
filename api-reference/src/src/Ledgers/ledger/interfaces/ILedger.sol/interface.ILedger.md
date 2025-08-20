# ILedger
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Ledgers/ledger/interfaces/ILedger.sol)

Interface for the Ledger diamond combining all facet functions


## Functions
### initialize

Initialize the ledger


```solidity
function initialize(address _owner, uint8 _decimals, string memory _currencyCode) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_owner`|`address`|The owner of this ledger|
|`_decimals`|`uint8`|The number of decimal places for amounts|
|`_currencyCode`|`string`|The ISO currency code for this ledger|


### owner

Get the owner of the ledger


```solidity
function owner() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The owner address|


### transferOwnership

Transfer ownership of the ledger


```solidity
function transferOwnership(address newOwner) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newOwner`|`address`|The new owner address|


### accountant

Get the accountant address


```solidity
function accountant() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The accountant address|


### setAccountant

Set the accountant address


```solidity
function setAccountant(address newAccountant) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newAccountant`|`address`|The new accountant address|


### decimals

Get the number of decimal places


```solidity
function decimals() external view returns (uint8);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|The decimal places|


### currencyCode

Get the currency code


```solidity
function currencyCode() external view returns (string memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|The ISO currency code|


### createAccountSection

Create a new account section


```solidity
function createAccountSection(string memory name, uint256 codeStart, uint256 codeEnd, bool debitIncreases)
    external
    returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|Section name|
|`codeStart`|`uint256`|Start of the code range|
|`codeEnd`|`uint256`|End of the code range|
|`debitIncreases`|`bool`|Whether debits increase account balances|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The ID of the newly created section|


### updateAccountSection

Update an existing account section


```solidity
function updateAccountSection(uint256 sectionId, string memory name, bool active) external;
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

Check if a section is debit-normal


```solidity
function isDebitNormal(uint256 sectionId) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`sectionId`|`uint256`|The section ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if debit increases the account balance|


### createAccount

Create a new account


```solidity
function createAccount(
    string memory code,
    string memory name,
    string memory description,
    uint256 sectionId,
    string memory parentCode,
    bool isParent,
    bool isContraAccount
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`code`|`string`|The account code using dot notation|
|`name`|`string`|The human-readable name of the account|
|`description`|`string`|The detailed description of the account's purpose|
|`sectionId`|`uint256`|The section this account belongs to|
|`parentCode`|`string`|The parent account code (empty string if top-level)|
|`isParent`|`bool`|Whether this account can have child accounts|
|`isContraAccount`|`bool`|Whether this is a contra account|


### updateAccount

Update an existing account's details


```solidity
function updateAccount(
    string memory code,
    string memory name,
    string memory description,
    bool active,
    bool isContraAccount
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`code`|`string`|The account code to update|
|`name`|`string`|The new name for the account|
|`description`|`string`|The new description for the account|
|`active`|`bool`|Whether the account should be active or inactive|
|`isContraAccount`|`bool`|Whether this is a contra account|


### reparentAccount

Reparent an account by creating a new account with proper code format


```solidity
function reparentAccount(string memory oldCode, string memory newParentCode, string memory newSuffix)
    external
    returns (string memory newCode);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`oldCode`|`string`|The current account code|
|`newParentCode`|`string`|The new parent account code (empty string for top-level)|
|`newSuffix`|`string`|The new suffix to append to the parent code|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`newCode`|`string`|The new account code|


### updateAccountParent

Update the parent of an existing account


```solidity
function updateAccountParent(string memory accountCode, string memory newParentCode) external returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`accountCode`|`string`|The account code to update|
|`newParentCode`|`string`|The new parent account code (empty string for top-level)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|Success status|


### toggleContraAccountStatus

Toggle the contra account status


```solidity
function toggleContraAccountStatus(string memory code) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`code`|`string`|The account code|


### setAccountParentStatus

Set account parent status


```solidity
function setAccountParentStatus(string memory code, bool isParent) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`code`|`string`|The account code|
|`isParent`|`bool`|Whether the account should be a parent|


### postTransaction

Post a balanced transaction with multiple entries


```solidity
function postTransaction(
    string[] calldata accountCodes,
    int256[] calldata amounts,
    string calldata description,
    uint8 status,
    uint256 transactionDate,
    uint256 postedDate
) external returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`accountCodes`|`string[]`|Array of account codes|
|`amounts`|`int256[]`|Array of corresponding amounts|
|`description`|`string`|Transaction description|
|`status`|`uint8`|Initial transaction status|
|`transactionDate`|`uint256`|Custom transaction date (use 0 for current block time)|
|`postedDate`|`uint256`|Custom posted date (only used if status is Posted)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The transaction ID|


### updateTransactionStatus

Update the status of a transaction


```solidity
function updateTransactionStatus(uint256 transactionId, TransactionStatus newStatus) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`transactionId`|`uint256`|The ID of the transaction to update|
|`newStatus`|`TransactionStatus`|The new status to set|


### getTransactionEntries

Get all entry IDs for a specific transaction


```solidity
function getTransactionEntries(uint256 transactionId) external view returns (uint256[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`transactionId`|`uint256`|The transaction ID to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256[]`|Array of entry IDs associated with the transaction|


### getTransaction

Get transaction details including status


```solidity
function getTransaction(uint256 transactionId)
    external
    view
    returns (uint256 id, uint256 transactionDate, string memory description, TransactionStatus status, uint256 postedAt);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`transactionId`|`uint256`|The transaction ID to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`id`|`uint256`|Transaction ID|
|`transactionDate`|`uint256`|Transaction date|
|`description`|`string`|Transaction description|
|`status`|`TransactionStatus`|Transaction status|
|`postedAt`|`uint256`|When the transaction was posted|


### getAccountBalance

Get the direct balance of an account (excluding children)


```solidity
function getAccountBalance(string memory code) external view returns (int256);
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

Get the total balance of an account including all its children recursively


```solidity
function getAccountTotalBalance(string memory code) external view returns (int256);
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

Get all direct child accounts of a parent account


```solidity
function getChildAccounts(string memory parentCode) external view returns (string[] memory);
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

Check if an account is debit-normal, considering contra accounts


```solidity
function isDebitNormalForAccount(string memory code) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`code`|`string`|The account code|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if debit increases the account balance|


### accounts

Get account information


```solidity
function accounts(string memory code)
    external
    view
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
function accountSections(uint256 sectionId) external view returns (LedgerStorage.AccountSectionInfo memory section);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`sectionId`|`uint256`|The section ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`section`|`LedgerStorage.AccountSectionInfo`|The account section struct|


### entries

Get entry information


```solidity
function entries(uint256 entryId) external view returns (LedgerStorage.Entry memory entry);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entryId`|`uint256`|The entry ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`entry`|`LedgerStorage.Entry`|The entry struct|


### transactions

Get transaction information


```solidity
function transactions(uint256 transactionId) external view returns (LedgerStorage.Transaction memory transaction);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`transactionId`|`uint256`|The transaction ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`transaction`|`LedgerStorage.Transaction`|The transaction struct|


### nextSectionId

Get the next section ID


```solidity
function nextSectionId() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The next section ID|


### nextEntryId

Get the next entry ID


```solidity
function nextEntryId() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The next entry ID|


### nextTransactionId

Get the next transaction ID


```solidity
function nextTransactionId() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The next transaction ID|


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

### EntryPosted

```solidity
event EntryPosted(uint256 entryId, string accountCode, int256 amount, string description, uint256 transactionId);
```

### TransactionCreated

```solidity
event TransactionCreated(
    uint256 transactionId, string description, TransactionStatus status, uint256 transactionDate, uint256 postedDate
);
```

### TransactionStatusUpdated

```solidity
event TransactionStatusUpdated(
    uint256 transactionId, TransactionStatus status, uint256 transactionDate, uint256 postedAt
);
```

### AccountantUpdated

```solidity
event AccountantUpdated(address newAccountant);
```

### LedgerInitialized

```solidity
event LedgerInitialized(address owner, uint8 decimals, string currencyCode);
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

### OwnershipTransferred

```solidity
event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
```

## Structs
### AccountSectionInfo

```solidity
struct AccountSectionInfo {
    uint256 id;
    string name;
    uint256 codeStart;
    uint256 codeEnd;
    bool debitNormal;
    bool active;
}
```

### Account

```solidity
struct Account {
    string code;
    string name;
    string description;
    int256 balance;
    uint256 sectionId;
    string parentCode;
    bool active;
    bool isParent;
    bool isContraAccount;
}
```

### Entry

```solidity
struct Entry {
    uint256 entryId;
    string accountCode;
    int256 amount;
    uint256 timestamp;
    uint256 transactionId;
    string description;
}
```

### Transaction

```solidity
struct Transaction {
    uint256 id;
    uint256 timestamp;
    string description;
    TransactionStatus status;
    uint256 postedAt;
}
```

### LedgerData

```solidity
struct LedgerData {
    uint8 decimals;
    string currencyCode;
    mapping(string => Account) accounts;
    mapping(uint256 => AccountSectionInfo) accountSections;
    uint256 nextSectionId;
    mapping(uint256 => Entry) entries;
    uint256 nextEntryId;
    mapping(uint256 => Transaction) transactions;
    uint256 nextTransactionId;
    mapping(uint256 => uint256[]) transactionEntries;
    mapping(string => string[]) childAccounts;
    address accountant;
    address owner;
    bool initialized;
}
```

## Enums
### TransactionStatus

```solidity
enum TransactionStatus {
    Pending,
    Posted,
    Archived
}
```

