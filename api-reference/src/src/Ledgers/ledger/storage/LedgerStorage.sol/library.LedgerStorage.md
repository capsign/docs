# LedgerStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Ledgers/ledger/storage/LedgerStorage.sol)

Storage library for the Ledger diamond containing all data structures

*Uses diamond storage pattern to avoid storage collisions*


## State Variables
### LEDGER_STORAGE_SLOT

```solidity
bytes32 private constant LEDGER_STORAGE_SLOT = keccak256("capsign.storage.ledger");
```


## Functions
### getLedgerStorage

Get the storage for the ledger


```solidity
function getLedgerStorage() internal pure returns (LedgerData storage s);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`s`|`LedgerData`|The storage struct|


### initializeLedgerStorage

Initialize the ledger storage


```solidity
function initializeLedgerStorage(address _owner, uint8 _decimals, string memory _currencyCode) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_owner`|`address`|The owner of this ledger|
|`_decimals`|`uint8`|The number of decimal places for amounts|
|`_currencyCode`|`string`|The ISO currency code for this ledger|


### isInitialized

Check if the ledger is initialized


```solidity
function isInitialized() internal view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if initialized|


### getOwner

Get the owner of the ledger


```solidity
function getOwner() internal view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The owner address|


### setOwner

Set the owner of the ledger


```solidity
function setOwner(address newOwner) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newOwner`|`address`|The new owner address|


### getAccountant

Get the accountant of the ledger


```solidity
function getAccountant() internal view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The accountant address|


### setAccountant

Set the accountant of the ledger


```solidity
function setAccountant(address newAccountant) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newAccountant`|`address`|The new accountant address|


### getDecimals

Get the decimals setting


```solidity
function getDecimals() internal view returns (uint8);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|The number of decimal places|


### getCurrencyCode

Get the currency code


```solidity
function getCurrencyCode() internal view returns (string memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|The ISO currency code|


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

