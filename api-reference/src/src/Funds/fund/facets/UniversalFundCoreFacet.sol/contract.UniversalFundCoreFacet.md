# UniversalFundCoreFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Funds/fund/facets/UniversalFundCoreFacet.sol)

**Author:**
CMX Protocol Team

Core fund facet handling initialization, status management, and basic operations

*This facet provides the fundamental fund functionality including:
- Fund initialization with configuration and component contracts
- Fund status management and lifecycle transitions
- Basic fund information and view functions
- Diamond-based access control (replacing AccessManaged)
- Emergency pause/unpause functionality
Key Features:
- Diamond storage pattern for state management
- Role-based access control without external dependencies
- Comprehensive fund configuration validation
- Automatic fund activation when target is reached
- Fund term management for closed-end funds
Access Control Roles:
- ENTITY_ROLE: Fund manager/GP with full operational control
- EMERGENCY_ADMIN_ROLE: Can pause/unpause fund operations*


## State Variables
### ENTITY_ROLE
Role identifier for fund entity/GP


```solidity
bytes32 public constant ENTITY_ROLE = keccak256("ENTITY_ROLE");
```


### EMERGENCY_ADMIN_ROLE
Role identifier for emergency admin


```solidity
bytes32 public constant EMERGENCY_ADMIN_ROLE = keccak256("EMERGENCY_ADMIN_ROLE");
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

### initialize

Initialize the fund with configuration and component contracts

*Only callable once - checks that name is not already set*


```solidity
function initialize(
    string memory _name,
    string memory _symbol,
    IUniversalFund.FundConfig memory _config,
    address _capitalAccountRegistry,
    address _distributionEngine,
    address _unitToken
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_name`|`string`|Human-readable name of the fund|
|`_symbol`|`string`|Symbol/ticker of the fund|
|`_config`|`IUniversalFund.FundConfig`|Fund configuration parameters|
|`_capitalAccountRegistry`|`address`|Address of capital account registry|
|`_distributionEngine`|`address`|Address of distribution engine|
|`_unitToken`|`address`|Address of fund unit token|


### setFundStatus

Update fund status with validation

*Only callable by entity role*


```solidity
function setFundStatus(IUniversalFund.FundStatus newStatus) external onlyRole(ENTITY_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newStatus`|`IUniversalFund.FundStatus`|The new status to transition to|


### extendFundTerm

Extend fund term for closed-end funds

*Only callable by entity role, subject to extension limits*


```solidity
function extendFundTerm(uint256 additionalMonths) external onlyRole(ENTITY_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`additionalMonths`|`uint256`|Number of months to extend the fund term|


### pause

Pause fund operations

*Only callable by emergency admin role*


```solidity
function pause() external onlyRole(EMERGENCY_ADMIN_ROLE);
```

### unpause

Unpause fund operations

*Only callable by emergency admin role*


```solidity
function unpause() external onlyRole(EMERGENCY_ADMIN_ROLE);
```

### grantRole

Grant a role to an account

*Only callable by entity role*


```solidity
function grantRole(bytes32 role, address account) external onlyRole(ENTITY_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`role`|`bytes32`|The role to grant|
|`account`|`address`|The account to grant the role to|


### revokeRole

Revoke a role from an account

*Only callable by entity role*


```solidity
function revokeRole(bytes32 role, address account) external onlyRole(ENTITY_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`role`|`bytes32`|The role to revoke|
|`account`|`address`|The account to revoke the role from|


### getFundConfig

Get fund configuration


```solidity
function getFundConfig() external view returns (IUniversalFund.FundConfig memory config);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`config`|`IUniversalFund.FundConfig`|The fund configuration struct|


### getFundStatus

Get current fund status


```solidity
function getFundStatus() external view returns (IUniversalFund.FundStatus status);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`status`|`IUniversalFund.FundStatus`|The current fund status|


### name

Get fund name


```solidity
function name() external view returns (string memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|name The fund name|


### symbol

Get fund symbol


```solidity
function symbol() external view returns (string memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|symbol The fund symbol|


### getTotalCommitments

Get total committed capital


```solidity
function getTotalCommitments() external view returns (uint256 totalCommitments);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalCommitments`|`uint256`|Total commitments from all investors|


### getTotalContributions

Get total contributed capital


```solidity
function getTotalContributions() external view returns (uint256 totalContributions);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalContributions`|`uint256`|Total contributions from all investors|


### getUnfundedCommitments

Get unfunded commitments


```solidity
function getUnfundedCommitments() external view returns (uint256 unfunded);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`unfunded`|`uint256`|Amount of committed but not yet contributed capital|


### getPortfolioValue

Get current portfolio value


```solidity
function getPortfolioValue() external view returns (uint256 portfolioValue);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`portfolioValue`|`uint256`|Current value of portfolio investments|


### canExtendTerm

Check if fund term can be extended


```solidity
function canExtendTerm() public view returns (bool canExtend);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`canExtend`|`bool`|True if the fund term can be extended|


### hasRole

Check if an account has a specific role


```solidity
function hasRole(bytes32 role, address account) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`role`|`bytes32`|The role to check|
|`account`|`address`|The account to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|hasRole True if the account has the role|


### paused

Check if fund is paused


```solidity
function paused() external view returns (bool isPaused);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`isPaused`|`bool`|True if the fund is paused|


### getAvailableBalance

Get available balance for investments


```solidity
function getAvailableBalance() external view returns (uint256 balance);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`balance`|`uint256`|Available balance in the fund|


### _setFundStatus

Internal function to update fund status with validation


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


### receive

Receive function to accept ETH

*Only accepts ETH if denomination asset is ETH (address(0))*


```solidity
receive() external payable;
```

## Events
### FundInitialized
Emitted when fund is initialized


```solidity
event FundInitialized(string name, string symbol, IUniversalFund.FundConfig config);
```

### FundStatusChanged
Emitted when fund status changes


```solidity
event FundStatusChanged(IUniversalFund.FundStatus oldStatus, IUniversalFund.FundStatus newStatus);
```

### FundTermExtended
Emitted when fund term is extended


```solidity
event FundTermExtended(uint256 newTermEndDate);
```

### Paused
Emitted when fund is paused


```solidity
event Paused(address account);
```

### Unpaused
Emitted when fund is unpaused


```solidity
event Unpaused(address account);
```

## Errors
### UnauthorizedAccess
Thrown when caller lacks required role


```solidity
error UnauthorizedAccess(address caller, bytes32 role);
```

### AlreadyInitialized
Thrown when fund is already initialized


```solidity
error AlreadyInitialized();
```

### InvalidConfiguration
Thrown when invalid configuration is provided


```solidity
error InvalidConfiguration(string reason);
```

### InvalidStatusTransition
Thrown when invalid status transition is attempted


```solidity
error InvalidStatusTransition(IUniversalFund.FundStatus from, IUniversalFund.FundStatus to);
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

