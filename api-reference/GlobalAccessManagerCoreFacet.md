# GlobalAccessManagerCoreFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Authorization/gam/facets/GlobalAccessManagerCoreFacet.sol)

*Core facet for GlobalAccessManager diamond that handles basic access manager functionality*


## State Variables
### ADMIN_ROLE

```solidity
uint64 public constant ADMIN_ROLE = 0;
```


### PROTOCOL_ADMIN_ROLE

```solidity
uint64 public constant PROTOCOL_ADMIN_ROLE = 1;
```


### REGISTRY_ADMIN_ROLE

```solidity
uint64 public constant REGISTRY_ADMIN_ROLE = 2;
```


### FACTORY_ADMIN_ROLE

```solidity
uint64 public constant FACTORY_ADMIN_ROLE = 3;
```


### SYSTEM_ATTESTOR_ROLE

```solidity
uint64 public constant SYSTEM_ATTESTOR_ROLE = 4;
```


### EMERGENCY_ADMIN_ROLE

```solidity
uint64 public constant EMERGENCY_ADMIN_ROLE = 5;
```


### ENTITY_ROLE

```solidity
uint64 public constant ENTITY_ROLE = 6;
```


### CRITICAL_DELAY

```solidity
uint32 public constant CRITICAL_DELAY = 2 days;
```


### STANDARD_DELAY

```solidity
uint32 public constant STANDARD_DELAY = 1 days;
```


### SHORT_DELAY

```solidity
uint32 public constant SHORT_DELAY = 6 hours;
```


### IMMEDIATE

```solidity
uint32 public constant IMMEDIATE = 0;
```


## Functions
### initialize

*Initialize the GlobalAccessManager diamond*


```solidity
function initialize(address _admin) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_admin`|`address`|The initial admin address|


### getProtocolRoles

*Get role information for UI/tooling*


```solidity
function getProtocolRoles()
    external
    pure
    returns (uint64[] memory roleIds, string[] memory roleNames, uint32[] memory delays);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`roleIds`|`uint64[]`|Array of all predefined role IDs|
|`roleNames`|`string[]`|Array of corresponding role names|
|`delays`|`uint32[]`|Array of execution delays for each role|


### registerProtocolContract

*Register a protocol contract*


```solidity
function registerProtocolContract(string calldata name, address contractAddress) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|The name of the contract|
|`contractAddress`|`address`|The address of the contract|


### setFactoryAuthorization

*Authorize or deauthorize a factory*


```solidity
function setFactoryAuthorization(address factory, bool authorized) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`factory`|`address`|The factory address|
|`authorized`|`bool`|Whether to authorize or deauthorize|


### getProtocolContract

*Get protocol contract address by name*


```solidity
function getProtocolContract(string calldata name) external view returns (address contractAddress);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|The contract name|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`contractAddress`|`address`|The contract address|


### isFactoryAuthorized

*Check if a factory is authorized*


```solidity
function isFactoryAuthorized(address factory) external view returns (bool authorized);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`factory`|`address`|The factory address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`authorized`|`bool`|True if the factory is authorized|


### getProtocolStats

*Get protocol statistics*


```solidity
function getProtocolStats() external view returns (uint256 totalEntities, uint256 totalFactories);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalEntities`|`uint256`|Total number of registered entities|
|`totalFactories`|`uint256`|Total number of authorized factories|


## Events
### GlobalAccessManagerDeployed

```solidity
event GlobalAccessManagerDeployed(address indexed admin);
```

### EmergencyActionExecuted

```solidity
event EmergencyActionExecuted(address indexed executor, bytes4 indexed selector);
```

### ProtocolContractRegistered

```solidity
event ProtocolContractRegistered(string indexed name, address indexed contractAddress);
```

### FactoryAuthorized

```solidity
event FactoryAuthorized(address indexed factory, bool authorized);
```

