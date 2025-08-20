# WalletGovernanceFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Wallets/wallet/facets/WalletGovernanceFacet.sol)

**Inherits:**
[MultiOwnable](/src/Wallets/wallet/MultiOwnable.sol/contract.MultiOwnable.md)

Manages governance contract integration and approval workflows

*Governance integration facet for diamond wallets*


## Functions
### setGovernanceContract

*Set the governance contract for this wallet*


```solidity
function setGovernanceContract(address governanceContract) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`governanceContract`|`address`|Address of the governance contract (0x0 to disable)|


### getGovernanceContract

*Get the governance contract for this wallet*


```solidity
function getGovernanceContract() external view returns (address governanceContract);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`governanceContract`|`address`|Address of the governance contract|


### requiresGovernanceApproval

*Check if an operation requires governance approval*


```solidity
function requiresGovernanceApproval(string calldata operation) external view returns (bool requiresApproval);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`operation`|`string`|The operation identifier|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`requiresApproval`|`bool`|True if governance approval is required|


### executeWithGovernance

*Execute a transaction with governance checks*


```solidity
function executeWithGovernance(address target, uint256 value, bytes calldata data, string calldata operation)
    external
    onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`target`|`address`|The target address|
|`value`|`uint256`|The ether value|
|`data`|`bytes`|The call data|
|`operation`|`string`|The operation description for governance|


### hasGovernanceApproval

*Check if governance has approved a specific transaction*


```solidity
function hasGovernanceApproval(address target, uint256 value, bytes calldata data, string calldata operation)
    external
    view
    returns (bool hasApproval);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`target`|`address`|The target address|
|`value`|`uint256`|The ether value|
|`data`|`bytes`|The call data|
|`operation`|`string`|The operation description|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`hasApproval`|`bool`|True if governance has approved this transaction|


### approveGovernanceTransaction

*Mark a transaction as approved by governance (called by governance contract)*


```solidity
function approveGovernanceTransaction(address target, uint256 value, bytes calldata data, string calldata operation)
    external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`target`|`address`|The target address|
|`value`|`uint256`|The ether value|
|`data`|`bytes`|The call data|
|`operation`|`string`|The operation description|


### revokeGovernanceApproval

*Revoke governance approval for a transaction*


```solidity
function revokeGovernanceApproval(address target, uint256 value, bytes calldata data, string calldata operation)
    external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`target`|`address`|The target address|
|`value`|`uint256`|The ether value|
|`data`|`bytes`|The call data|
|`operation`|`string`|The operation description|


### _isGovernanceRequiredOperation

*Check if an operation requires governance approval*


```solidity
function _isGovernanceRequiredOperation(string calldata operation) internal pure returns (bool required);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`operation`|`string`|The operation identifier|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`required`|`bool`|True if governance approval is required|


### _hasGovernanceApproval

*Check if governance has approved a specific transaction*


```solidity
function _hasGovernanceApproval(address target, uint256 value, bytes calldata data, string calldata operation)
    internal
    view
    returns (bool hasApproval);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`target`|`address`|The target address|
|`value`|`uint256`|The ether value|
|`data`|`bytes`|The call data|
|`operation`|`string`|The operation description|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`hasApproval`|`bool`|True if governance has approved this transaction|


## Events
### GovernanceContractUpdated

```solidity
event GovernanceContractUpdated(address indexed oldGovernanceContract, address indexed newGovernanceContract);
```

### GovernanceApprovalRequired

```solidity
event GovernanceApprovalRequired(address indexed governance, string operation, bytes callData);
```

### GovernanceTransactionExecuted

```solidity
event GovernanceTransactionExecuted(
    address indexed governance, address indexed target, uint256 value, string operation
);
```

## Errors
### GovernanceApprovalMissing

```solidity
error GovernanceApprovalMissing(address governance, string operation, bytes callData);
```

### InvalidGovernanceContract

```solidity
error InvalidGovernanceContract();
```

### GovernanceNotSet

```solidity
error GovernanceNotSet();
```

### UnauthorizedGovernanceAccess

```solidity
error UnauthorizedGovernanceAccess();
```

