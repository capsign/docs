# LedgerDeploymentFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Ledgers/factory/facets/LedgerDeploymentFacet.sol)

Facet providing ledger deployment functionality for LedgerFactory diamond

*Handles the creation and initialization of Ledger instances using stored bytecode*


## State Variables
### LEDGER_DEPLOYMENT_STORAGE_SLOT

```solidity
bytes32 private constant LEDGER_DEPLOYMENT_STORAGE_SLOT = keccak256("ledger.deployment.storage");
```


## Functions
### deployDiamond

Deploy a diamond using FactoryBase pattern


```solidity
function deployDiamond(IDiamond.FacetCut[] memory cuts, bytes memory initData, bytes32 salt)
    external
    returns (address diamondAddress);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`cuts`|`IDiamond.FacetCut[]`|Array of facet cuts to install|
|`initData`|`bytes`|Initialization data for the diamond|
|`salt`|`bytes32`|Salt for deterministic deployment|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`diamondAddress`|`address`|The address of the deployed diamond|


### setLedgerBytecode

Set the bytecode for Ledger deployment


```solidity
function setLedgerBytecode(bytes calldata bytecode) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`bytecode`|`bytes`|The creation bytecode of the Ledger contract|


### getLedgerBytecode

Get the bytecode for Ledger deployment


```solidity
function getLedgerBytecode() external view returns (bytes memory bytecode);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`bytecode`|`bytes`|The creation bytecode|


### createLedger

Creates a new Ledger instance for a user with default settings (2 decimals, USD)

*Each user is limited to exactly one ledger*


```solidity
function createLedger() external returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The address of the newly created Ledger|


### createLedger

Creates a new Ledger instance for a user with custom currency settings

*Each user is limited to exactly one ledger*


```solidity
function createLedger(uint8 _decimals, string memory _currencyCode) public returns (address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_decimals`|`uint8`|The number of decimal places (e.g., 2 for cents)|
|`_currencyCode`|`string`|The ISO currency code (e.g., "USD", "EUR")|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The address of the newly created Ledger|


### getTotalLedgerCount

Gets the total number of ledgers created by this factory


```solidity
function getTotalLedgerCount() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The total count of created ledgers|


### getLedgerByUser

Gets the ledger address for a specific user


```solidity
function getLedgerByUser(address user) external view returns (address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the user|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The address of the user's ledger|


### getUserLedger

Gets the caller's ledger address


```solidity
function getUserLedger() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The address of the caller's ledger|


### getLedgerInfo

Gets ledger information by index


```solidity
function getLedgerInfo(uint256 index) external view returns (LedgerInfo memory ledgerInfo);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`index`|`uint256`|The index of the ledger|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`ledgerInfo`|`LedgerInfo`|The ledger information|


### isInitialized

Check if the deployment facet is initialized


```solidity
function isInitialized() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if initialized|


### _getLedgerDeploymentStorage

Get the storage for this facet


```solidity
function _getLedgerDeploymentStorage() internal pure returns (LedgerDeploymentStorage storage s);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`s`|`LedgerDeploymentStorage`|The storage struct|


## Events
### LedgerCreated

```solidity
event LedgerCreated(address indexed ledgerAddress, address indexed owner, uint8 decimals, string currencyCode);
```

### LedgerFactoryCreated

```solidity
event LedgerFactoryCreated(address factoryAddress);
```

### LedgerBytecodeSet

```solidity
event LedgerBytecodeSet(uint256 bytecodeLength);
```

## Errors
### UserAlreadyHasLedger

```solidity
error UserAlreadyHasLedger();
```

### LedgerDeploymentFailed

```solidity
error LedgerDeploymentFailed();
```

### BytecodeNotSet

```solidity
error BytecodeNotSet();
```

## Structs
### LedgerDeploymentStorage

```solidity
struct LedgerDeploymentStorage {
    address initialized;
    mapping(uint256 => LedgerInfo) ledgers;
    uint256 ledgerCount;
    mapping(address => address) userLedgers;
    bytes ledgerBytecode;
}
```

### LedgerInfo

```solidity
struct LedgerInfo {
    address ledgerAddress;
    address owner;
    uint256 createdAt;
    uint8 decimals;
    string currencyCode;
}
```

