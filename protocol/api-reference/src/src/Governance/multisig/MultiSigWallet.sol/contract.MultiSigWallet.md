# MultiSigWallet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Governance/multisig/MultiSigWallet.sol)

Traditional N-of-M multi-signature wallet for entity control

*Simple threshold signatures without roles or complex approval logic*


## State Variables
### entityWallet

```solidity
address public entityWallet;
```


### owners

```solidity
address[] public owners;
```


### required

```solidity
uint256 public required;
```


### isOwner

```solidity
mapping(address => bool) public isOwner;
```


### transactions

```solidity
Transaction[] public transactions;
```


## Functions
### onlyOwner


```solidity
modifier onlyOwner();
```

### txExists


```solidity
modifier txExists(uint256 _txIndex);
```

### notExecuted


```solidity
modifier notExecuted(uint256 _txIndex);
```

### notConfirmed


```solidity
modifier notConfirmed(uint256 _txIndex);
```

### constructor


```solidity
constructor(address _entityWallet, address[] memory _owners, uint256 _required);
```

### submitTransaction

Submit a transaction for multi-sig approval


```solidity
function submitTransaction(address _target, uint256 _value, bytes memory _data)
    public
    onlyOwner
    returns (uint256 txIndex);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_target`|`address`|Address to call|
|`_value`|`uint256`|Amount of ETH to send|
|`_data`|`bytes`|Transaction data|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`txIndex`|`uint256`|Index of the submitted transaction|


### confirmTransaction

Confirm a pending transaction


```solidity
function confirmTransaction(uint256 _txIndex) public onlyOwner txExists(_txIndex) notConfirmed(_txIndex);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_txIndex`|`uint256`|Index of transaction to confirm|


### revokeConfirmation

Revoke confirmation for a transaction


```solidity
function revokeConfirmation(uint256 _txIndex) public onlyOwner txExists(_txIndex) notExecuted(_txIndex);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_txIndex`|`uint256`|Index of transaction|


### executeTransaction

Execute a confirmed transaction


```solidity
function executeTransaction(uint256 _txIndex) public onlyOwner txExists(_txIndex) notExecuted(_txIndex);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_txIndex`|`uint256`|Index of transaction to execute|


### addOwner

Add a new owner (requires multi-sig approval)


```solidity
function addOwner(address owner) public onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|Address of new owner|


### removeOwner

Remove an owner (requires multi-sig approval)


```solidity
function removeOwner(address owner) public onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|Address to remove|


### changeRequirement

Change the number of required confirmations


```solidity
function changeRequirement(uint256 _required) public onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_required`|`uint256`|New required confirmation count|


### _addOwner


```solidity
function _addOwner(address owner) external;
```

### _removeOwner


```solidity
function _removeOwner(address owner) external;
```

### _changeRequirement


```solidity
function _changeRequirement(uint256 _required) external;
```

### getOwners

Get list of owners


```solidity
function getOwners() public view returns (address[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Array of owner addresses|


### getTransactionCount

Get transaction count


```solidity
function getTransactionCount() public view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Number of transactions|


### getTransaction

Get transaction details


```solidity
function getTransaction(uint256 _txIndex)
    public
    view
    returns (address target, uint256 value, bytes memory data, bool executed, uint256 confirmationCount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_txIndex`|`uint256`|Transaction index|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`target`|`address`|Target address|
|`value`|`uint256`|Transaction value|
|`data`|`bytes`|Transaction data|
|`executed`|`bool`|Whether executed|
|`confirmationCount`|`uint256`|Number of confirmations|


### isConfirmed

Check if transaction is confirmed by owner


```solidity
function isConfirmed(uint256 _txIndex, address _owner) public view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_txIndex`|`uint256`|Transaction index|
|`_owner`|`address`|Owner address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|Whether transaction is confirmed by owner|


### getPendingTransactions

Get pending transactions (not yet executed)


```solidity
function getPendingTransactions() public view returns (uint256[] memory indexes);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`indexes`|`uint256[]`|Array of pending transaction indexes|


### canExecute

Check if transaction has enough confirmations to execute


```solidity
function canExecute(uint256 _txIndex) public view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_txIndex`|`uint256`|Transaction index|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|Whether transaction can be executed|


## Events
### Deposit

```solidity
event Deposit(address indexed sender, uint256 amount, uint256 balance);
```

### SubmitTransaction

```solidity
event SubmitTransaction(
    address indexed owner, uint256 indexed txIndex, address indexed target, uint256 value, bytes data
);
```

### ConfirmTransaction

```solidity
event ConfirmTransaction(address indexed owner, uint256 indexed txIndex);
```

### RevokeConfirmation

```solidity
event RevokeConfirmation(address indexed owner, uint256 indexed txIndex);
```

### ExecuteTransaction

```solidity
event ExecuteTransaction(address indexed owner, uint256 indexed txIndex);
```

### OwnerAdded

```solidity
event OwnerAdded(address indexed owner);
```

### OwnerRemoved

```solidity
event OwnerRemoved(address indexed owner);
```

### RequirementChanged

```solidity
event RequirementChanged(uint256 required);
```

## Structs
### Transaction

```solidity
struct Transaction {
    address target;
    uint256 value;
    bytes data;
    bool executed;
    uint256 confirmationCount;
    mapping(address => bool) confirmations;
}
```

