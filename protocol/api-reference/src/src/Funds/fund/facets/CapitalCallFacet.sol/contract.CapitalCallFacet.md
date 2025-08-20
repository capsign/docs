# CapitalCallFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Funds/fund/facets/CapitalCallFacet.sol)

**Inherits:**
ReentrancyGuard

**Author:**
CMX Protocol Team

Facet handling capital commitments, capital calls, and investor contributions

*This facet manages the fund's capital raising and deployment lifecycle including:
- Investor capital commitments during fundraising
- Capital call issuance to funded investors
- Investor contribution processing with management fee deduction
- Unit token issuance for LP interests
- Automatic fund activation when target is reached
Key Features:
- Pro-rata capital call calculations
- Management fee deduction from contributions
- GP/LP differentiation for unit token issuance
- Automatic capital call completion tracking
- Comprehensive event emission for subgraph integration
Access Control:
- Capital commitments: Restricted to placement agents or entity
- Capital call issuance: Restricted to entity role only
- Contributions: Open to committed investors*


## State Variables
### ENTITY_ROLE
Role identifier for fund entity/GP


```solidity
bytes32 public constant ENTITY_ROLE = keccak256("ENTITY_ROLE");
```


### PLACEMENT_AGENT_ROLE
Role identifier for placement agents


```solidity
bytes32 public constant PLACEMENT_AGENT_ROLE = keccak256("PLACEMENT_AGENT_ROLE");
```


## Functions
### onlyRole


```solidity
modifier onlyRole(bytes32 role);
```

### whenNotPaused


```solidity
modifier whenNotPaused();
```

### onlyStatus


```solidity
modifier onlyStatus(IUniversalFund.FundStatus requiredStatus);
```

### onlyInvestor


```solidity
modifier onlyInvestor();
```

### commitCapital

Commit capital to the fund during fundraising phase

*Restricted to placement agents or entity, creates investor accounts and issues unit tokens*


```solidity
function commitCapital(address investor, uint256 amount)
    external
    onlyRole(PLACEMENT_AGENT_ROLE)
    onlyStatus(IUniversalFund.FundStatus.FUNDRAISING)
    whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|Address of the investor making the commitment|
|`amount`|`uint256`|Amount of capital being committed|


### issueCapitalCall

Issue a capital call to all committed investors

*Only callable by entity, validates against unfunded commitments*


```solidity
function issueCapitalCall(uint256 amount, uint256 dueDate, string memory purposeURI)
    external
    onlyRole(ENTITY_ROLE)
    onlyStatus(IUniversalFund.FundStatus.ACTIVE)
    returns (uint256 callNumber);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|Total amount to call from all investors|
|`dueDate`|`uint256`|Deadline for investor contributions|
|`purposeURI`|`string`|IPFS URI describing the purpose of the capital call|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`callNumber`|`uint256`|Unique identifier for this capital call|


### contributeCapital

Contribute capital in response to a capital call

*Calculates pro-rata share, deducts management fees, updates accounts*


```solidity
function contributeCapital(uint256 callNumber) external payable onlyInvestor nonReentrant whenNotPaused;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`callNumber`|`uint256`|The capital call number to contribute to|


### getCapitalCall

Get capital call details


```solidity
function getCapitalCall(uint256 callNumber)
    external
    view
    returns (uint256 totalAmount, uint256 dueDate, uint256 purposeHash, bool isComplete);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`callNumber`|`uint256`|The capital call number to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalAmount`|`uint256`|Total amount of the capital call|
|`dueDate`|`uint256`|Deadline for contributions|
|`purposeHash`|`uint256`|Hash of the purpose document|
|`isComplete`|`bool`|Whether the call has been completed|


### hasInvestorPaidCall

Check if an investor has paid a specific capital call


```solidity
function hasInvestorPaidCall(uint256 callNumber, address investor) external view returns (bool hasPaid);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`callNumber`|`uint256`|The capital call number|
|`investor`|`address`|The investor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`hasPaid`|`bool`|True if the investor has paid the call|


### getCurrentCallNumber

Get current capital call number


```solidity
function getCurrentCallNumber() external view returns (uint256 callNumber);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`callNumber`|`uint256`|The current capital call number|


### isInvestor

Check if an address is an investor


```solidity
function isInvestor(address account) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|isInvestor True if the address is an investor|


### getInvestorShare

Get investor's pro-rata share for a capital call


```solidity
function getInvestorShare(address investor, uint256 callAmount) external view returns (uint256 share);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`callAmount`|`uint256`|The total capital call amount|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`share`|`uint256`|The investor's pro-rata share|


### _checkCallCompletion

Internal function to check capital call completion

*Marks call as complete when 95% of expected contributions are received*


```solidity
function _checkCallCompletion(uint256 callNumber) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`callNumber`|`uint256`|The capital call number to check|


### _setFundStatus

Internal function to update fund status


```solidity
function _setFundStatus(IUniversalFund.FundStatus newStatus) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newStatus`|`IUniversalFund.FundStatus`|The new status to transition to|


### _checkRole

Internal function to check if an account has a role


```solidity
function _checkRole(bytes32 role, address account) internal view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`role`|`bytes32`|The role to check|
|`account`|`address`|The account to check|


## Events
### InvestorCommitted
Emitted when an investor commits capital


```solidity
event InvestorCommitted(address indexed investor, uint256 commitment);
```

### CapitalCallIssued
Emitted when a capital call is issued


```solidity
event CapitalCallIssued(uint256 indexed callNumber, uint256 amount, uint256 dueDate);
```

### CapitalContributed
Emitted when an investor contributes capital


```solidity
event CapitalContributed(address indexed investor, uint256 indexed callNumber, uint256 amount);
```

### CapitalCallCompleted
Emitted when a capital call is completed


```solidity
event CapitalCallCompleted(uint256 indexed callNumber, uint256 totalReceived);
```

## Errors
### UnauthorizedAccess
Thrown when caller lacks required role


```solidity
error UnauthorizedAccess(address caller, bytes32 role);
```

### FundPaused
Thrown when operations are attempted while paused


```solidity
error FundPaused();
```

### InvalidFundStatus
Thrown when fund is not in required status


```solidity
error InvalidFundStatus(IUniversalFund.FundStatus required, IUniversalFund.FundStatus actual);
```

### InvalidParameters
Thrown when invalid parameters are provided


```solidity
error InvalidParameters(string reason);
```

### IncorrectPayment
Thrown when payment amount is incorrect


```solidity
error IncorrectPayment(uint256 expected, uint256 actual);
```

### NotInvestor
Thrown when caller is not an investor


```solidity
error NotInvestor(address caller);
```

### AlreadyPaid
Thrown when investor has already paid a capital call


```solidity
error AlreadyPaid(address investor, uint256 callNumber);
```

### CallExpired
Thrown when capital call has expired


```solidity
error CallExpired(uint256 callNumber, uint256 dueDate);
```

