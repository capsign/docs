# IdentityAccessManagerSubscriptionFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Authorization/iam/facets/IdentityAccessManagerSubscriptionFacet.sol)

*Subscription management facet for IdentityAccessManager diamond*


## Functions
### setSubscriptionManager

*Set the subscription manager contract*


```solidity
function setSubscriptionManager(address _subscriptionManager) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_subscriptionManager`|`address`|Address of the subscription manager|


### setSubscriptionRequirement

*Update subscription requirements*


```solidity
function setSubscriptionRequirement(uint8 _requiredTier, bool _enforced) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_requiredTier`|`uint8`|Minimum subscription tier required|
|`_enforced`|`bool`|Whether to enforce subscription requirements|


### getSubscriptionStatus

*Check subscription status for the entity*


```solidity
function getSubscriptionStatus() external view returns (bool hasSubscription, uint8 currentTier);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`hasSubscription`|`bool`|True if entity has required subscription|
|`currentTier`|`uint8`|The entity's current subscription tier (if available)|


### getSubscriptionConfig

*Get subscription configuration*


```solidity
function getSubscriptionConfig() external view returns (address manager, uint8 requiredTier, bool enforced);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`manager`|`address`|Current subscription manager address|
|`requiredTier`|`uint8`|Required subscription tier|
|`enforced`|`bool`|Whether subscriptions are enforced|


### isRoleAvailableWithSubscription

*Check if a role is available considering subscription requirements*


```solidity
function isRoleAvailableWithSubscription(uint64, address account) external view returns (bool available);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint64`||
|`account`|`address`|The account to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`available`|`bool`|True if the role is available for the account|


### subscribe

*Subscribe to a tier using CMX tokens*


```solidity
function subscribe(uint8 tier) external payable;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`uint8`|The subscription tier to subscribe to|


### cancelSubscription

*Cancel current subscription*


```solidity
function cancelSubscription() external;
```

### renewSubscription

*Renew current subscription*


```solidity
function renewSubscription(uint8 tier) external payable;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`uint8`|The tier to renew (can upgrade/downgrade)|


### getPaymentAmount

*Get payment amount for a subscription tier*


```solidity
function getPaymentAmount(uint8 tier, address paymentToken) external view returns (uint256 amount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`uint8`|The subscription tier|
|`paymentToken`|`address`|The token to pay with (address(0) for CMX)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The payment amount required|


### _canManageSubscriptions

*Check if caller can manage subscription settings*


```solidity
function _canManageSubscriptions() internal view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if caller is entity admin or global protocol admin|


## Events
### SubscriptionManagerUpdated

```solidity
event SubscriptionManagerUpdated(address indexed oldManager, address indexed newManager);
```

### SubscriptionRequirementUpdated

```solidity
event SubscriptionRequirementUpdated(uint8 oldTier, uint8 newTier, bool enforced);
```

### SubscriptionPurchased

```solidity
event SubscriptionPurchased(address indexed entity, uint8 tier, uint256 amount);
```

### SubscriptionCancelled

```solidity
event SubscriptionCancelled(address indexed entity);
```

### SubscriptionRenewed

```solidity
event SubscriptionRenewed(address indexed entity, uint8 tier, uint256 amount);
```

