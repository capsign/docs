# ISimpleApprovalWorkflow
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Governance/approval/interfaces/ISimpleApprovalWorkflow.sol)

Interface for simple approval-based governance


## Functions
### requestApproval

*Request approval for an operation*


```solidity
function requestApproval(string calldata operation, bytes calldata callData) external returns (uint256 requestId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`operation`|`string`|Description of the operation|
|`callData`|`bytes`|The call data to execute if approved|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`requestId`|`uint256`|The ID of the approval request|


### approve

*Approve a pending request*


```solidity
function approve(uint256 requestId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requestId`|`uint256`|The request ID to approve|


### reject

*Reject a pending request*


```solidity
function reject(uint256 requestId, string calldata reason) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requestId`|`uint256`|The request ID to reject|
|`reason`|`string`|Reason for rejection|


### executeApproval

*Execute an approved request*


```solidity
function executeApproval(uint256 requestId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requestId`|`uint256`|The request ID to execute|


### updateApprover

*Update the approver address*


```solidity
function updateApprover(address newApprover) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newApprover`|`address`|New approver address|


### getApprovalRequest

*Get approval request details*


```solidity
function getApprovalRequest(uint256 requestId) external view returns (ApprovalRequest memory request);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requestId`|`uint256`|The request ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`request`|`ApprovalRequest`|The approval request details|


### getPendingRequests

*Get all pending requests*


```solidity
function getPendingRequests() external view returns (uint256[] memory requestIds);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`requestIds`|`uint256[]`|Array of pending request IDs|


### getApprover

*Get the current approver*


```solidity
function getApprover() external view returns (address approver);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`approver`|`address`|The current approver address|


### canExecute

*Check if a request can be executed*


```solidity
function canExecute(uint256 requestId) external view returns (bool canExecute);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requestId`|`uint256`|The request ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`canExecute`|`bool`|True if the request can be executed|


## Events
### ApprovalRequested

```solidity
event ApprovalRequested(uint256 indexed requestId, address indexed requester, string operation);
```

### ApprovalGranted

```solidity
event ApprovalGranted(uint256 indexed requestId, address indexed approver);
```

### ApprovalRejected

```solidity
event ApprovalRejected(uint256 indexed requestId, address indexed approver, string reason);
```

### ApprovalExecuted

```solidity
event ApprovalExecuted(uint256 indexed requestId, address indexed executor);
```

### ApprovalExpired

```solidity
event ApprovalExpired(uint256 indexed requestId);
```

### ApproverUpdated

```solidity
event ApproverUpdated(address indexed oldApprover, address indexed newApprover);
```

## Structs
### ApprovalRequest

```solidity
struct ApprovalRequest {
    uint256 id;
    address requester;
    string operation;
    bytes callData;
    uint256 timestamp;
    uint256 expiryTime;
    ApprovalStatus status;
    address approver;
    string rejectionReason;
}
```

## Enums
### ApprovalStatus

```solidity
enum ApprovalStatus {
    Pending,
    Approved,
    Rejected,
    Executed,
    Expired
}
```

