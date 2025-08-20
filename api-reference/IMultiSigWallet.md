# IMultiSigWallet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Governance/multisig/interfaces/IMultiSigWallet.sol)

Interface for multi-signature wallet governance


## Functions
### submitTransaction

*Submit a transaction for approval*


```solidity
function submitTransaction(address destination, uint256 value, bytes calldata data)
    external
    returns (uint256 transactionId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`destination`|`address`|The target address|
|`value`|`uint256`|The ether value|
|`data`|`bytes`|The call data|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`transactionId`|`uint256`|The ID of the submitted transaction|


### confirmTransaction

*Confirm a transaction*


```solidity
function confirmTransaction(uint256 transactionId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`transactionId`|`uint256`|The transaction ID to confirm|


### revokeConfirmation

*Revoke confirmation for a transaction*


```solidity
function revokeConfirmation(uint256 transactionId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`transactionId`|`uint256`|The transaction ID to revoke confirmation for|


### executeTransaction

*Execute a confirmed transaction*


```solidity
function executeTransaction(uint256 transactionId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`transactionId`|`uint256`|The transaction ID to execute|


### cancelTransaction

*Cancel a pending transaction*


```solidity
function cancelTransaction(uint256 transactionId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`transactionId`|`uint256`|The transaction ID to cancel|


### addSigner

*Add a new signer*


```solidity
function addSigner(address signer) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`signer`|`address`|The signer address to add|


### removeSigner

*Remove a signer*


```solidity
function removeSigner(address signer) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`signer`|`address`|The signer address to remove|


### changeRequiredSignatures

*Change the required number of signatures*


```solidity
function changeRequiredSignatures(uint256 required) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`required`|`uint256`|New required signature count|


### getTransaction

*Get transaction details*


```solidity
function getTransaction(uint256 transactionId) external view returns (Transaction memory transaction);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`transactionId`|`uint256`|The transaction ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`transaction`|`Transaction`|The transaction details|


### getPendingTransactions

*Get pending transactions*


```solidity
function getPendingTransactions() external view returns (uint256[] memory transactionIds);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`transactionIds`|`uint256[]`|Array of pending transaction IDs|


### getConfirmations

*Get transaction confirmations*


```solidity
function getConfirmations(uint256 transactionId) external view returns (address[] memory signers);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`transactionId`|`uint256`|The transaction ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`signers`|`address[]`|Array of addresses that confirmed the transaction|


### isConfirmed

*Check if transaction is confirmed*


```solidity
function isConfirmed(uint256 transactionId) external view returns (bool confirmed);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`transactionId`|`uint256`|The transaction ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`confirmed`|`bool`|True if transaction has enough confirmations|


### getSigners

*Get all signers*


```solidity
function getSigners() external view returns (address[] memory signers);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`signers`|`address[]`|Array of signer addresses|


### getRequiredSignatures

*Get required signature count*


```solidity
function getRequiredSignatures() external view returns (uint256 required);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`required`|`uint256`|The required number of signatures|


### isSigner

*Check if an address is a signer*


```solidity
function isSigner(address signer) external view returns (bool isSigner);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`signer`|`address`|The address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`isSigner`|`bool`|True if the address is a signer|


## Events
### TransactionSubmitted

```solidity
event TransactionSubmitted(uint256 indexed transactionId, address indexed submitter);
```

### TransactionConfirmed

```solidity
event TransactionConfirmed(uint256 indexed transactionId, address indexed signer);
```

### TransactionRevoked

```solidity
event TransactionRevoked(uint256 indexed transactionId, address indexed signer);
```

### TransactionExecuted

```solidity
event TransactionExecuted(uint256 indexed transactionId);
```

### TransactionCancelled

```solidity
event TransactionCancelled(uint256 indexed transactionId);
```

### SignerAdded

```solidity
event SignerAdded(address indexed signer);
```

### SignerRemoved

```solidity
event SignerRemoved(address indexed signer);
```

### RequiredSignaturesChanged

```solidity
event RequiredSignaturesChanged(uint256 oldRequired, uint256 newRequired);
```

## Structs
### Transaction

```solidity
struct Transaction {
    uint256 id;
    address destination;
    uint256 value;
    bytes data;
    bool executed;
    uint256 confirmationCount;
    uint256 timestamp;
    TransactionStatus status;
}
```

## Enums
### TransactionStatus

```solidity
enum TransactionStatus {
    Pending,
    Executed,
    Cancelled
}
```

