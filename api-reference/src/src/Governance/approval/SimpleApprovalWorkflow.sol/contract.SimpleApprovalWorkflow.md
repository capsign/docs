# SimpleApprovalWorkflow
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Governance/approval/SimpleApprovalWorkflow.sol)

Basic approval workflow for entity wallets

*Supports role-based approvals and executive limits without complex governance*


## State Variables
### companyName

```solidity
string public companyName;
```


### entityWallet

```solidity
address public entityWallet;
```


### approvalRules

```solidity
mapping(bytes4 => ApprovalRule) public approvalRules;
```


### approvers

```solidity
mapping(address => Approver) public approvers;
```


### approverList

```solidity
address[] public approverList;
```


### pendingApprovals

```solidity
mapping(uint256 => PendingApproval) public pendingApprovals;
```


### nextApprovalId

```solidity
uint256 public nextApprovalId;
```


### requireDualApproval

```solidity
bool public requireDualApproval;
```


### defaultTimelock

```solidity
uint256 public defaultTimelock;
```


### approvalExpiry

```solidity
uint256 public approvalExpiry;
```


## Functions
### constructor


```solidity
constructor(address _entityWallet, string memory _companyName, bool _requireDualApproval);
```

### requestApproval

Request approval for a transaction


```solidity
function requestApproval(address target, bytes calldata data, uint256 value) external returns (uint256 approvalId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`target`|`address`|Address to call|
|`data`|`bytes`|Transaction data|
|`value`|`uint256`|Transaction value|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`approvalId`|`uint256`|ID of the approval request|


### grantApproval

Grant approval for a pending transaction


```solidity
function grantApproval(uint256 approvalId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`approvalId`|`uint256`|ID of the approval to approve|


### executeApproval

Execute an approved transaction


```solidity
function executeApproval(uint256 approvalId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`approvalId`|`uint256`|ID of the approval to execute|


### addApprover

Add a new approver


```solidity
function addApprover(address approver, string memory name, string memory role, uint256 individualLimit) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`approver`|`address`|Address of the approver|
|`name`|`string`|Display name|
|`role`|`string`|Role (CEO, CFO, etc.)|
|`individualLimit`|`uint256`|Amount they can approve alone|


### removeApprover

Remove an approver


```solidity
function removeApprover(address approver) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`approver`|`address`|Address to remove|


### setApprovalRule

Update approval rule for a transaction type


```solidity
function setApprovalRule(bytes4 selector, uint256 maxAmount, uint256 requiredCount, uint256 timelock) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`selector`|`bytes4`|Function selector|
|`maxAmount`|`uint256`|Maximum amount for this rule|
|`requiredCount`|`uint256`|Number of approvals required|
|`timelock`|`uint256`|Timelock in seconds|


### getApprovalStatus

Get approval status for a pending transaction


```solidity
function getApprovalStatus(uint256 approvalId)
    external
    view
    returns (PendingApproval memory approval, bool canExecute);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`approvalId`|`uint256`|ID of the approval|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`approval`|`PendingApproval`|The approval details|
|`canExecute`|`bool`|Whether it can be executed now|


### getActiveApprovers

Get all active approvers


```solidity
function getActiveApprovers() external view returns (address[] memory addresses, Approver[] memory details);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`addresses`|`address[]`|Array of active approver addresses|
|`details`|`Approver[]`|Array of approver details|


### _executeImmediately


```solidity
function _executeImmediately(address target, bytes calldata data, uint256 value)
    internal
    returns (uint256 approvalId);
```

### _executeApproval


```solidity
function _executeApproval(uint256 approvalId) internal;
```

### _getRequiredApprovals


```solidity
function _getRequiredApprovals(PendingApproval memory approval) internal view returns (uint256);
```

### _getActiveApprovers


```solidity
function _getActiveApprovers() internal view returns (address[] memory);
```

### _isAdmin


```solidity
function _isAdmin(address account) internal view returns (bool);
```

### _setDefaultApprovalRules


```solidity
function _setDefaultApprovalRules() internal;
```

## Events
### ApprovalRequested

```solidity
event ApprovalRequested(
    uint256 indexed approvalId, address indexed requester, address indexed target, uint256 value, bytes4 selector
);
```

### ApprovalGranted

```solidity
event ApprovalGranted(
    uint256 indexed approvalId, address indexed approver, uint256 approvalsCount, uint256 requiredCount
);
```

### TransactionExecuted

```solidity
event TransactionExecuted(uint256 indexed approvalId, address indexed executor, bool success);
```

### ApproverAdded

```solidity
event ApproverAdded(address indexed approver, string role, uint256 individualLimit);
```

### ApproverRemoved

```solidity
event ApproverRemoved(address indexed approver);
```

### ApprovalRuleUpdated

```solidity
event ApprovalRuleUpdated(bytes4 indexed selector, uint256 maxAmount, uint256 requiredCount);
```

## Errors
### UnauthorizedApprover

```solidity
error UnauthorizedApprover();
```

### InsufficientApprovals

```solidity
error InsufficientApprovals();
```

### ApprovalExpired

```solidity
error ApprovalExpired();
```

### ApprovalAlreadyExecuted

```solidity
error ApprovalAlreadyExecuted();
```

### ExceedsIndividualLimit

```solidity
error ExceedsIndividualLimit();
```

### InvalidApprovalRule

```solidity
error InvalidApprovalRule();
```

### DuplicateApproval

```solidity
error DuplicateApproval();
```

## Structs
### ApprovalRule

```solidity
struct ApprovalRule {
    uint256 maxAmount;
    address[] requiredApprovers;
    uint256 requiredCount;
    uint256 timelock;
}
```

### PendingApproval

```solidity
struct PendingApproval {
    address wallet;
    address target;
    bytes data;
    uint256 value;
    address requester;
    uint256 requestTime;
    address[] approvers;
    bool executed;
    uint256 expiryTime;
}
```

### Approver

```solidity
struct Approver {
    string name;
    string role;
    uint256 individualLimit;
    bool isActive;
}
```

