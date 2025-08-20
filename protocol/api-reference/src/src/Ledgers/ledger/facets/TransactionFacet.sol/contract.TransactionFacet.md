# TransactionFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Ledgers/ledger/facets/TransactionFacet.sol)

Facet for managing transactions and entries in the ledger

*Handles transaction posting, status updates, and entry management*

*Now includes event bubbling to diamond level for subgraph indexing*


## Functions
### onlyOwnerOrAccountant


```solidity
modifier onlyOwnerOrAccountant();
```

### onlyInitialized


```solidity
modifier onlyInitialized();
```

### postTransaction

Posts a balanced transaction with multiple entries


```solidity
function postTransaction(
    string[] calldata accountCodes,
    int256[] calldata amounts,
    string calldata description,
    uint8 status,
    uint256 transactionDate,
    uint256 postedDate
) external onlyOwnerOrAccountant onlyInitialized returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`accountCodes`|`string[]`|Array of account codes|
|`amounts`|`int256[]`|Array of corresponding amounts (can be positive or negative)|
|`description`|`string`|Transaction description|
|`status`|`uint8`|Initial transaction status (defaults to Pending if 0)|
|`transactionDate`|`uint256`|Custom transaction date (use 0 for current block time)|
|`postedDate`|`uint256`|Custom posted date (only used if status is Posted, use 0 for current block time)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The transaction ID|


### updateTransactionStatus

Updates the status of a transaction


```solidity
function updateTransactionStatus(uint256 transactionId, LedgerStorage.TransactionStatus newStatus)
    external
    onlyOwnerOrAccountant
    onlyInitialized;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`transactionId`|`uint256`|The ID of the transaction to update|
|`newStatus`|`LedgerStorage.TransactionStatus`|The new status to set|


### getTransactionEntries

Gets all entry IDs for a specific transaction


```solidity
function getTransactionEntries(uint256 transactionId) external view onlyInitialized returns (uint256[] memory);
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

Gets transaction details including status


```solidity
function getTransaction(uint256 transactionId)
    external
    view
    onlyInitialized
    returns (
        uint256 id,
        uint256 transactionDate,
        string memory description,
        LedgerStorage.TransactionStatus status,
        uint256 postedAt
    );
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`transactionId`|`uint256`|The transaction ID to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`id`|`uint256`|Transaction ID|
|`transactionDate`|`uint256`|Transaction date (can be backdated)|
|`description`|`string`|Transaction description|
|`status`|`LedgerStorage.TransactionStatus`|Transaction status|
|`postedAt`|`uint256`|When the transaction was posted (0 if not yet posted)|


### entries

Get entry information


```solidity
function entries(uint256 entryId) external view onlyInitialized returns (LedgerStorage.Entry memory entry);
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
function transactions(uint256 transactionId)
    external
    view
    onlyInitialized
    returns (LedgerStorage.Transaction memory transaction);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`transactionId`|`uint256`|The transaction ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`transaction`|`LedgerStorage.Transaction`|The transaction struct|


### nextEntryId

Get the next entry ID


```solidity
function nextEntryId() external view onlyInitialized returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The next entry ID|


### nextTransactionId

Get the next transaction ID


```solidity
function nextTransactionId() external view onlyInitialized returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The next transaction ID|


## Events
### EntryPosted

```solidity
event EntryPosted(uint256 entryId, string accountCode, int256 amount, string description, uint256 transactionId);
```

### TransactionCreated

```solidity
event TransactionCreated(
    uint256 transactionId,
    string description,
    LedgerStorage.TransactionStatus status,
    uint256 transactionDate,
    uint256 postedDate
);
```

### TransactionStatusUpdated

```solidity
event TransactionStatusUpdated(
    uint256 transactionId, LedgerStorage.TransactionStatus status, uint256 transactionDate, uint256 postedAt
);
```

