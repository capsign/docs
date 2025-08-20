# IdentityAccessManagerPaymasterFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Authorization/iam/facets/IdentityAccessManagerPaymasterFacet.sol)

**Inherits:**
[IFactoryCMXPaymaster](/src/Diamonds/factory/IFactoryCMXPaymaster.sol/interface.IFactoryCMXPaymaster.md)

Provides entity-sponsored gas payments for authorized users

*Integrates with existing CMX paymaster system for flexible payment options*


## Functions
### onlyEntityAdmin


```solidity
modifier onlyEntityAdmin();
```

### paymasterEnabled


```solidity
modifier paymasterEnabled();
```

### setPaymasterEnabled

Enable or disable entity paymaster


```solidity
function setPaymasterEnabled(bool enabled) external onlyEntityAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`enabled`|`bool`|Whether to enable paymaster|


### depositForGasSponsorship

Deposit ETH for gas sponsorship

*Entity admin can deposit ETH to sponsor gas for authorized users*


```solidity
function depositForGasSponsorship() external payable onlyEntityAdmin;
```

### withdrawFromPaymaster

Withdraw ETH from paymaster balance


```solidity
function withdrawFromPaymaster(uint256 amount, address recipient) external onlyEntityAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|Amount to withdraw|
|`recipient`|`address`|Withdrawal recipient|


### getPaymasterBalance

Get current paymaster balance


```solidity
function getPaymasterBalance() external view returns (uint256 balance);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`balance`|`uint256`|Current ETH balance available for gas sponsorship|


### isPaymasterEnabled

Check if paymaster is enabled


```solidity
function isPaymasterEnabled() external view returns (bool enabled);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`enabled`|`bool`|Whether paymaster is enabled|


### setSponsoredUser

Add or update sponsored user


```solidity
function setSponsoredUser(address user, uint256 dailyLimit) external onlyEntityAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User address to sponsor|
|`dailyLimit`|`uint256`|Daily spending limit for user (0 = unlimited)|


### removeSponsoredUser

Remove user from sponsored list


```solidity
function removeSponsoredUser(address user) external onlyEntityAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User address to remove|


### isSponsoredUser

Check if user is sponsored


```solidity
function isSponsoredUser(address user) external view returns (bool sponsored);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`sponsored`|`bool`|Whether user is sponsored|


### getUserSpendingInfo

Get user's daily spending info


```solidity
function getUserSpendingInfo(address user) external view returns (uint256 limit, uint256 spent, uint256 remaining);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`limit`|`uint256`|Daily limit|
|`spent`|`uint256`|Amount spent today|
|`remaining`|`uint256`|Amount remaining today|


### setOperationSponsorship

Configure operation sponsorship


```solidity
function setOperationSponsorship(bytes4 operation, bool sponsored, uint256 estimatedCost) external onlyEntityAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`operation`|`bytes4`|Operation selector to sponsor|
|`sponsored`|`bool`|Whether to sponsor this operation|
|`estimatedCost`|`uint256`|Estimated gas cost for this operation|


### isOperationSponsored

Check if operation is sponsored


```solidity
function isOperationSponsored(bytes4 operation) external view returns (bool sponsored);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`operation`|`bytes4`|Operation selector|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`sponsored`|`bool`|Whether operation is sponsored|


### setDailyBudget

Set total daily budget for gas sponsorship


```solidity
function setDailyBudget(uint256 budget) external onlyEntityAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`budget`|`uint256`|Daily budget in ETH|


### getBudgetInfo

Get budget information


```solidity
function getBudgetInfo() external view returns (uint256 budget, uint256 spent, uint256 remaining);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`budget`|`uint256`|Total daily budget|
|`spent`|`uint256`|Amount spent today|
|`remaining`|`uint256`|Amount remaining today|


### validateSponsorship

Validate if a user operation can be sponsored


```solidity
function validateSponsorship(address user, bytes4 operation, uint256 estimatedCost)
    external
    view
    paymasterEnabled
    returns (bool canSponsor, string memory reason);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User requesting sponsorship|
|`operation`|`bytes4`|Operation selector|
|`estimatedCost`|`uint256`|Estimated gas cost|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`canSponsor`|`bool`|Whether operation can be sponsored|
|`reason`|`string`|Reason if cannot sponsor|


### recordSponsoredOperation

Record sponsored operation (called after successful sponsorship)


```solidity
function recordSponsoredOperation(address user, bytes4 operation, uint256 actualCost) external paymasterEnabled;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User whose operation was sponsored|
|`operation`|`bytes4`|Operation selector|
|`actualCost`|`uint256`|Actual gas cost|


### updatePaymasterSupport

Update paymaster support (IFactoryPaymaster interface)


```solidity
function updatePaymasterSupport(address paymaster, bool supported) external override onlyEntityAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`paymaster`|`address`|Paymaster address|
|`supported`|`bool`|Whether to support this paymaster|


### isPaymasterSupported

Check if a paymaster is supported (IFactoryPaymaster interface)


```solidity
function isPaymasterSupported(address paymaster) external view override returns (bool supported);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`paymaster`|`address`|Paymaster address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`supported`|`bool`|Whether the paymaster is supported|


### getSupportedPaymasters

Get all supported paymasters (IFactoryPaymaster interface)


```solidity
function getSupportedPaymasters() external view override returns (address[] memory paymasters);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`paymasters`|`address[]`|Array of supported paymaster addresses|


### preValidatePayment

Pre-validate payment before operation (IFactoryPaymaster interface)


```solidity
function preValidatePayment(address user, address paymaster, PackedUserOperation calldata userOp)
    external
    view
    override
    returns (bool validated);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User who will pay|
|`paymaster`|`address`|Paymaster address|
|`userOp`|`PackedUserOperation`|User operation (for gas estimation)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`validated`|`bool`|Whether the payment is pre-validated|


### updateCMXPaymasterSupport

Update CMX paymaster support


```solidity
function updateCMXPaymasterSupport(address paymaster, bool supported) external override onlyEntityAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`paymaster`|`address`|CMX paymaster address|
|`supported`|`bool`|Whether to support this paymaster|


### isCMXPaymasterSupported

Check if a CMX paymaster is supported


```solidity
function isCMXPaymasterSupported(address paymaster) external view override returns (bool supported);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`paymaster`|`address`|CMX paymaster address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`supported`|`bool`|Whether the paymaster is supported|


### getSupportedCMXPaymasters

Get all supported CMX paymasters


```solidity
function getSupportedCMXPaymasters() external view override returns (address[] memory paymasters);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`paymasters`|`address[]`|Array of supported paymaster addresses|


### validateCMXPayment

Validate CMX paymaster can handle the fee for an operation


```solidity
function validateCMXPayment(address user, address paymaster, string calldata operationType)
    external
    view
    override
    returns (bool canPay, uint256 cmxRequired, uint256 ethEquivalent);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User who will pay the fee|
|`paymaster`|`address`|CMX paymaster address|
|`operationType`|`string`|Type of operation (for fee calculation)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`canPay`|`bool`|Whether the user can pay via CMX|
|`cmxRequired`|`uint256`|CMX amount required|
|`ethEquivalent`|`uint256`|ETH equivalent amount|


### preValidateCMXPayment

Pre-validate CMX payment before operation


```solidity
function preValidateCMXPayment(address user, address paymaster, PackedUserOperation calldata userOp)
    external
    view
    override
    returns (bool validated);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User who will pay|
|`paymaster`|`address`|CMX paymaster address|
|`userOp`|`PackedUserOperation`|User operation (for gas estimation)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`validated`|`bool`|Whether the payment is pre-validated|


### setCMXPaymasterEnabled

Enable/disable CMX paymaster integration


```solidity
function setCMXPaymasterEnabled(bool enabled) external onlyEntityAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`enabled`|`bool`|Whether to enable CMX paymaster integration|


### isCMXPaymasterEnabled

Check if CMX paymaster integration is enabled


```solidity
function isCMXPaymasterEnabled() external view returns (bool enabled);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`enabled`|`bool`|Whether CMX paymaster integration is enabled|


### getBaseFee

Get base fee for operation type


```solidity
function getBaseFee(string calldata operationType) external pure override returns (uint256 fee);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`operationType`|`string`|Type of operation|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`fee`|`uint256`|Base fee in ETH (wei) - IAM operations are typically free|


### calculateTotalFee

Calculate total fee including any factory-specific multipliers


```solidity
function calculateTotalFee(string calldata operationType, uint256 baseAmount)
    external
    pure
    override
    returns (uint256 totalFee);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`operationType`|`string`|Type of operation|
|`baseAmount`|`uint256`|Base amount in ETH|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalFee`|`uint256`|Total fee including multipliers - IAM operations are typically free|


### _canManageEntitySettings

Check if caller can manage entity settings


```solidity
function _canManageEntitySettings() internal view virtual returns (bool canManage);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`canManage`|`bool`|Whether caller can manage settings|


### _getUserDailySpent

Get user's spending for current day


```solidity
function _getUserDailySpent(address user) internal view returns (uint256 spent);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`spent`|`uint256`|Amount spent today|


### _getTotalDailySpent

Get total spending for current day


```solidity
function _getTotalDailySpent() internal view returns (uint256 spent);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`spent`|`uint256`|Total amount spent today|


## Events
### PaymasterStatusUpdated
Emitted when paymaster is enabled/disabled for entity


```solidity
event PaymasterStatusUpdated(address indexed entity, bool enabled);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|Entity address|
|`enabled`|`bool`|Whether paymaster is enabled|

### PaymasterDeposit
Emitted when entity deposits ETH for gas sponsorship


```solidity
event PaymasterDeposit(address indexed entity, uint256 amount, uint256 newBalance);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|Entity address|
|`amount`|`uint256`|ETH amount deposited|
|`newBalance`|`uint256`|New paymaster balance|

### PaymasterWithdrawal
Emitted when entity withdraws ETH from paymaster


```solidity
event PaymasterWithdrawal(address indexed entity, uint256 amount, address indexed recipient);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|Entity address|
|`amount`|`uint256`|ETH amount withdrawn|
|`recipient`|`address`|Withdrawal recipient|

### SponsoredUserUpdated
Emitted when a user is added/removed from sponsored list


```solidity
event SponsoredUserUpdated(address indexed entity, address indexed user, bool sponsored, uint256 dailyLimit);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|Entity address|
|`user`|`address`|User address|
|`sponsored`|`bool`|Whether user is sponsored|
|`dailyLimit`|`uint256`|Daily spending limit for user|

### OperationSponsored
Emitted when an operation is sponsored


```solidity
event OperationSponsored(address indexed entity, address indexed user, bytes4 indexed operation, uint256 gasCost);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|Entity address|
|`user`|`address`|Sponsored user|
|`operation`|`bytes4`|Operation selector|
|`gasCost`|`uint256`|Gas cost covered|

### BudgetUpdated
Emitted when budget limits are updated


```solidity
event BudgetUpdated(address indexed entity, uint256 dailyBudget);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|Entity address|
|`dailyBudget`|`uint256`|New daily budget|

### OperationSponsorshipUpdated
Emitted when operation sponsorship is configured


```solidity
event OperationSponsorshipUpdated(
    address indexed entity, bytes4 indexed operation, bool sponsored, uint256 estimatedCost
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`entity`|`address`|Entity address|
|`operation`|`bytes4`|Operation selector|
|`sponsored`|`bool`|Whether operation is sponsored|
|`estimatedCost`|`uint256`|Estimated cost for operation|

## Errors
### PaymasterNotEnabled

```solidity
error PaymasterNotEnabled();
```

### InsufficientPaymasterBalance

```solidity
error InsufficientPaymasterBalance(uint256 required, uint256 available);
```

### UserNotSponsored

```solidity
error UserNotSponsored(address user);
```

### OperationNotSponsored

```solidity
error OperationNotSponsored(bytes4 operation);
```

### DailyLimitExceeded

```solidity
error DailyLimitExceeded(address user, uint256 limit, uint256 attempted);
```

### BudgetExceeded

```solidity
error BudgetExceeded(uint256 budget, uint256 attempted);
```

### InvalidDailyLimit

```solidity
error InvalidDailyLimit(uint256 limit);
```

### InvalidBudget

```solidity
error InvalidBudget(uint256 budget);
```

### OnlyEntityAdmin

```solidity
error OnlyEntityAdmin();
```

### WithdrawalFailed

```solidity
error WithdrawalFailed();
```

