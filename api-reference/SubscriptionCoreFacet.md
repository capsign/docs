# SubscriptionCoreFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Billing/facets/SubscriptionCoreFacet.sol)

**Inherits:**
ReentrancyGuard

Core subscription functionality for the SubscriptionManager diamond

*Handles subscription creation, renewal, and cancellation*

*Now includes event bubbling to diamond level for subgraph indexing*


## Functions
### subscribeCMX

Subscribes to a tier using CMX tokens directly (convenience function)


```solidity
function subscribeCMX(uint8 tierLevel) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tierLevel`|`uint8`|The tier level to subscribe to|


### subscribe

Subscribes to a tier using any accepted token


```solidity
function subscribe(uint8 tierLevel, address paymentToken) external nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tierLevel`|`uint8`|The tier level to subscribe to|
|`paymentToken`|`address`|The token to pay with (CMX or stablecoin)|


### subscribeWithETH

Subscribes to a tier using ETH (for future implementation)


```solidity
function subscribeWithETH(uint8 tierLevel) external payable nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tierLevel`|`uint8`|The tier level to subscribe to|


### renewSubscription

Renews a subscription using the same token as last payment


```solidity
function renewSubscription() external nonReentrant;
```

### renewSubscription

Renews a subscription with a specific token


```solidity
function renewSubscription(address paymentToken) external nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`paymentToken`|`address`|The token to pay with|


### changeTierCMX

Changes subscription tier using CMX (convenience function)


```solidity
function changeTierCMX(uint8 newTierLevel) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newTierLevel`|`uint8`|The new tier level|


### changeTier

Changes subscription tier


```solidity
function changeTier(uint8 newTierLevel, address paymentToken) external nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newTierLevel`|`uint8`|The new tier level|
|`paymentToken`|`address`|The token to pay with|


### cancelSubscription

Cancels a subscription


```solidity
function cancelSubscription() external;
```

### hasActiveSubscription

Check if an address has an active subscription at or above specified tier


```solidity
function hasActiveSubscription(address subscriber, uint8 minTierLevel) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subscriber`|`address`|The subscriber address|
|`minTierLevel`|`uint8`|Minimum tier level required|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if subscriber has active subscription at or above the tier|


### getPaymentAmount

Calculates the payment amount for a subscriber and tier with a specific token


```solidity
function getPaymentAmount(address subscriber, uint8 tierLevel, address paymentToken) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subscriber`|`address`|The subscriber address|
|`tierLevel`|`uint8`|The tier level|
|`paymentToken`|`address`|The token to pay with|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|uint256 The payment amount in the token's decimals|


### _subscribeInternal

*Internal subscribe function*


```solidity
function _subscribeInternal(uint8 tierLevel, address paymentToken) internal;
```

### _changeTierInternal

*Internal change tier function*


```solidity
function _changeTierInternal(uint8 newTierLevel, address paymentToken) internal;
```

### _createSubscription

*Create or update subscription*


```solidity
function _createSubscription(address subscriber, uint8 tierLevel, uint256 amount, address paymentToken) internal;
```

### _calculatePaymentAmount

*Calculate payment amount with discounts*


```solidity
function _calculatePaymentAmount(address subscriber, uint8 tierLevel, address paymentToken)
    internal
    view
    returns (uint256);
```

### _getStorage

*Get storage layout*


```solidity
function _getStorage() internal pure returns (SubscriptionManagerStorage.Layout storage);
```

## Events
### SubscriptionCreated

```solidity
event SubscriptionCreated(
    address indexed subscriber, uint8 tierLevel, uint256 paymentAmount, uint256 nextDue, address paymentToken
);
```

### SubscriptionRenewed

```solidity
event SubscriptionRenewed(
    address indexed subscriber, uint8 tierLevel, uint256 paymentAmount, uint256 nextDue, address paymentToken
);
```

### SubscriptionCancelled

```solidity
event SubscriptionCancelled(address indexed subscriber);
```

### SubscriptionTierUpdated

```solidity
event SubscriptionTierUpdated(address indexed subscriber, uint8 oldTier, uint8 newTier);
```

## Errors
### InvalidTierLevel

```solidity
error InvalidTierLevel();
```

### TierNotActive

```solidity
error TierNotActive();
```

### PaymentTokenNotAccepted

```solidity
error PaymentTokenNotAccepted();
```

### NoActiveSubscription

```solidity
error NoActiveSubscription();
```

### FreeTierRequiresCMXWithDiscount

```solidity
error FreeTierRequiresCMXWithDiscount();
```

### ETHPaymentsNotSupported

```solidity
error ETHPaymentsNotSupported();
```

