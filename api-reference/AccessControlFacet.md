# AccessControlFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Ledgers/ledger/facets/AccessControlFacet.sol)

Facet for managing access control and initialization in the ledger

*Handles owner/accountant management and ledger initialization*


## Functions
### onlyOwner


```solidity
modifier onlyOwner();
```

### onlyInitialized


```solidity
modifier onlyInitialized();
```

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


### _createStandardSections

Create standard account sections during initialization


```solidity
function _createStandardSections() internal;
```

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
function transferOwnership(address newOwner) external onlyOwner onlyInitialized;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newOwner`|`address`|The new owner address|


### accountant

Get the accountant address


```solidity
function accountant() external view onlyInitialized returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The accountant address|


### setAccountant

Set the accountant address


```solidity
function setAccountant(address newAccountant) external onlyOwner onlyInitialized;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newAccountant`|`address`|The new accountant address|


### isInitialized

Check if the ledger is initialized


```solidity
function isInitialized() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if initialized|


### isAuthorized

Check if an address is authorized (owner or accountant)


```solidity
function isAuthorized(address account) external view onlyInitialized returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if authorized|


### isCallerAuthorized

Check if the caller is authorized


```solidity
function isCallerAuthorized() external view onlyInitialized returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if caller is authorized|


### requireAuthorized

Require that the caller is authorized (owner or accountant)


```solidity
function requireAuthorized() external view onlyInitialized;
```

### requireOwner

Require that the caller is the owner


```solidity
function requireOwner() external view onlyInitialized;
```

## Events
### AccountantUpdated

```solidity
event AccountantUpdated(address newAccountant);
```

### LedgerInitialized

```solidity
event LedgerInitialized(address owner, uint8 decimals, string currencyCode);
```

### OwnershipTransferred

```solidity
event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
```

