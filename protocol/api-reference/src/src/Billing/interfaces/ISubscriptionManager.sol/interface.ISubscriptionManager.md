# ISubscriptionManager
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Billing/interfaces/ISubscriptionManager.sol)

Interface for the SubscriptionManager diamond

*Defines all functions available on the SubscriptionManager diamond*


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
function subscribe(uint8 tierLevel, address paymentToken) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tierLevel`|`uint8`|The tier level to subscribe to|
|`paymentToken`|`address`|The token to pay with (CMX or stablecoin)|


### subscribeWithETH

Subscribes to a tier using ETH (for future implementation)


```solidity
function subscribeWithETH(uint8 tierLevel) external payable;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tierLevel`|`uint8`|The tier level to subscribe to|


### renewSubscription

Renews a subscription using the same token as last payment


```solidity
function renewSubscription() external;
```

### renewSubscription

Renews a subscription with a specific token


```solidity
function renewSubscription(address paymentToken) external;
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
function changeTier(uint8 newTierLevel, address paymentToken) external;
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


### getUserTier

Get the current tier level for a subscriber


```solidity
function getUserTier(address subscriber) external view returns (uint8);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subscriber`|`address`|The subscriber address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|tierLevel The subscriber's current tier level (0 if no active subscription)|


### getSubscriptionDetails

Get subscription details for a subscriber


```solidity
function getSubscriptionDetails(address subscriber)
    external
    view
    returns (
        uint8 tierLevel,
        uint256 nextPaymentDue,
        bool isActive,
        uint256 lastPaymentAmount,
        address lastPaymentToken
    );
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subscriber`|`address`|The subscriber address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tierLevel`|`uint8`|The subscriber's current tier level|
|`nextPaymentDue`|`uint256`|When the next payment is due|
|`isActive`|`bool`|Whether the subscription is active|
|`lastPaymentAmount`|`uint256`|The last payment amount|
|`lastPaymentToken`|`address`|The token used for last payment|


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


### initialize

Initialize the subscription manager


```solidity
function initialize(address _cmxToken, address _swapRouter, address _treasury, address _initialOwner) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_cmxToken`|`address`|Address of the CMX token|
|`_swapRouter`|`address`|Address of the Uniswap V3 SwapRouter|
|`_treasury`|`address`|Address to receive payments|
|`_initialOwner`|`address`|Address that will become the owner of the contract|


### setTier

Creates or updates a subscription tier


```solidity
function setTier(uint8 tierLevel, string calldata name, uint256 monthlyFeeUSD, bool isActive) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tierLevel`|`uint8`|The tier level|
|`name`|`string`|The tier name|
|`monthlyFeeUSD`|`uint256`|The monthly fee in USD (6 decimals)|
|`isActive`|`bool`|Whether the tier is active|


### addAcceptedToken

Adds a new payment token


```solidity
function addAcceptedToken(address token, uint8 decimals, bool isStablecoin) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The token address to add|
|`decimals`|`uint8`|The token decimals|
|`isStablecoin`|`bool`|Whether this is a stablecoin (true) or CMX (false)|


### removeAcceptedToken

Removes a token from the accepted tokens list


```solidity
function removeAcceptedToken(address token) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The token address to remove|


### setCMXDiscount

Sets the CMX discount percentage


```solidity
function setCMXDiscount(uint256 discountBasisPoints) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`discountBasisPoints`|`uint256`|Discount in basis points (e.g., 2000 = 20%)|


### setAddressTierDiscount

Sets a customer-specific discount for a specific tier (only applies to CMX payments)


```solidity
function setAddressTierDiscount(address subscriber, uint8 tierLevel, uint256 discountBasisPoints) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subscriber`|`address`|The subscriber address|
|`tierLevel`|`uint8`|The tier level|
|`discountBasisPoints`|`uint256`|Discount in basis points (10000 = 100%)|


### adminSetSubscription

Admin function to set a subscription as active for a specific period


```solidity
function adminSetSubscription(address subscriber, uint8 tierLevel, uint256 durationInSeconds) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subscriber`|`address`|The subscriber address|
|`tierLevel`|`uint8`|The tier level|
|`durationInSeconds`|`uint256`|Duration of subscription|


### adminCancelSubscription

Admin function to force cancel a subscription


```solidity
function adminCancelSubscription(address subscriber) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subscriber`|`address`|The subscriber address|


### setTreasury

Updates the treasury address


```solidity
function setTreasury(address newTreasury) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newTreasury`|`address`|New treasury address|


### setUniswapPoolFee

Sets the Uniswap pool fee for token swaps (for future ETH support)


```solidity
function setUniswapPoolFee(uint24 newFee) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newFee`|`uint24`|New fee tier (e.g., 3000 for 0.3%)|


### getSubscriptionInfo

Get full subscription info for a subscriber


```solidity
function getSubscriptionInfo(address subscriber)
    external
    view
    returns (
        uint8 tierLevel,
        uint256 nextPaymentDue,
        bool isActive,
        uint256 lastPaymentAmount,
        string memory tierName,
        uint256 tierFeeUSD,
        address lastPaymentToken
    );
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subscriber`|`address`|The subscriber address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tierLevel`|`uint8`|Current tier level|
|`nextPaymentDue`|`uint256`|Next payment due timestamp|
|`isActive`|`bool`|Whether subscription is active and not expired|
|`lastPaymentAmount`|`uint256`|Last payment amount|
|`tierName`|`string`|Name of current tier|
|`tierFeeUSD`|`uint256`|Monthly fee for current tier in USD (6 decimals)|
|`lastPaymentToken`|`address`|Token used for last payment|


### getTierInfo

Get tier information


```solidity
function getTierInfo(uint8 tierLevel)
    external
    view
    returns (string memory name, uint256 monthlyFeeUSD, bool isActive);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tierLevel`|`uint8`|The tier level|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|The tier name|
|`monthlyFeeUSD`|`uint256`|The monthly fee in USD (6 decimals)|
|`isActive`|`bool`|Whether the tier is active|


### getTokenConfig

Get token configuration for a specific token


```solidity
function getTokenConfig(address token) external view returns (bool accepted, uint8 decimals, bool isStablecoin);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The token address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`accepted`|`bool`|Whether the token is accepted|
|`decimals`|`uint8`|Token decimals|
|`isStablecoin`|`bool`|Whether it's a stablecoin|


### getAddressTierDiscount

Gets the discount for a specific address and tier


```solidity
function getAddressTierDiscount(address subscriber, uint8 tierLevel) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subscriber`|`address`|The subscriber address|
|`tierLevel`|`uint8`|The tier level|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|discountBasisPoints Discount in basis points|


### getCMXDiscount

Get the CMX discount percentage


```solidity
function getCMXDiscount() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|discountBasisPoints Discount in basis points|


### getCMXToken

Get the CMX token address


```solidity
function getCMXToken() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|cmxToken The CMX token address|


### getTreasury

Get the treasury address


```solidity
function getTreasury() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|treasury The treasury address|


### getSwapRouter

Get the swap router address


```solidity
function getSwapRouter() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|swapRouter The swap router address|


### getUniswapPoolFee

Get the Uniswap pool fee


```solidity
function getUniswapPoolFee() external view returns (uint24);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint24`|poolFee The pool fee|


### isInitialized

Check if the contract is initialized


```solidity
function isInitialized() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|initialized Whether the contract is initialized|


### cmxToken

Get the CMX token address (convenience function)


```solidity
function cmxToken() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The CMX token address|


### tiers

Get tier information (convenience function)


```solidity
function tiers(uint8 tierLevel) external view returns (string memory name, uint256 monthlyFeeUSD, bool isActive);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tierLevel`|`uint8`|The tier level|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|The tier name|
|`monthlyFeeUSD`|`uint256`|The monthly fee in USD (6 decimals)|
|`isActive`|`bool`|Whether the tier is active|


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

### TierCreated

```solidity
event TierCreated(uint8 tierLevel, string name, uint256 monthlyFeeUSD);
```

### TierUpdated

```solidity
event TierUpdated(uint8 tierLevel, string name, uint256 monthlyFeeUSD, bool isActive);
```

### CMXDiscountUpdated

```solidity
event CMXDiscountUpdated(uint256 oldDiscount, uint256 newDiscount);
```

### AcceptedTokenAdded

```solidity
event AcceptedTokenAdded(address indexed token, uint8 decimals, bool isStablecoin);
```

### AcceptedTokenRemoved

```solidity
event AcceptedTokenRemoved(address indexed token);
```

### TreasuryUpdated

```solidity
event TreasuryUpdated(address indexed oldTreasury, address indexed newTreasury);
```

### UniswapPoolFeeUpdated

```solidity
event UniswapPoolFeeUpdated(uint24 oldFee, uint24 newFee);
```

