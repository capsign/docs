# AssetAdminFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/facets/AssetAdminFacet.sol)

Facet for administrative functions including access control and pausing

*Diamond facet providing administrative functionality*


## Functions
### _authority


```solidity
function _authority() internal view returns (address);
```

### addAgent

Add a new agent with admin privileges


```solidity
function addAgent(address _agent) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_agent`|`address`|Address of the agent to add|


### removeAgent

Remove an agent


```solidity
function removeAgent(address _agent) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_agent`|`address`|Address of the agent to remove|


### isAgent

Check if an address is an agent


```solidity
function isAgent(address _agent) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_agent`|`address`|Address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the address is an agent, false otherwise|


### setPaused

Set the paused state


```solidity
function setPaused(bool _paused) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_paused`|`bool`|The new paused state|


### emergencyPause

Emergency pause function - can only pause, not unpause


```solidity
function emergencyPause() external;
```

### getAdminInfo

Get all administrative information for an address


```solidity
function getAdminInfo(address account) external view returns (bool isOwner, bool isAgentRole, bool isAdmin);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`isOwner`|`bool`|Whether the address is the owner|
|`isAgentRole`|`bool`|Whether the address is an agent|
|`isAdmin`|`bool`|Whether the address has admin privileges (owner or agent)|


### batchAddAgents

Batch add multiple agents


```solidity
function batchAddAgents(address[] calldata agents) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`agents`|`address[]`|Array of addresses to add as agents|


### batchRemoveAgents

Batch remove multiple agents


```solidity
function batchRemoveAgents(address[] calldata agents) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`agents`|`address[]`|Array of addresses to remove as agents|


### isInitialized

Check if the contract is initialized


```solidity
function isInitialized() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the contract is initialized|


### getContractStatus

Get contract status information


```solidity
function getContractStatus()
    external
    view
    returns (bool contractPaused, address ownerAddress, uint256 totalAgents, bool contractInitialized);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`contractPaused`|`bool`|Whether the contract is paused|
|`ownerAddress`|`address`|The current owner address|
|`totalAgents`|`uint256`|The total number of agents|
|`contractInitialized`|`bool`|Whether the contract is initialized|


### hasAdminRole

Check if an address has any administrative privileges


```solidity
function hasAdminRole(address account) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the address has admin privileges|


### emergencyTransferOwnership

Emergency function to transfer ownership (requires current owner)


```solidity
function emergencyTransferOwnership(address newOwner) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newOwner`|`address`|The new owner address|


### resetAllAgents

Reset all agents (emergency function)

*This removes all agents but preserves the owner*


```solidity
function resetAllAgents() external view;
```

### batchCheckAdminStatus

Check multiple addresses for admin status


```solidity
function batchCheckAdminStatus(address[] calldata accounts) external view returns (bool[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`accounts`|`address[]`|Array of addresses to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool[]`|Array of booleans indicating admin status for each address|


## Events
### AgentAdded

```solidity
event AgentAdded(address indexed agent);
```

### AgentRemoved

```solidity
event AgentRemoved(address indexed agent);
```

### PausedSet

```solidity
event PausedSet(bool indexed paused);
```

