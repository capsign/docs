# SubscriptionManagerStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Billing/storage/SubscriptionManagerStorage.sol)

Contains all storage variables for the subscription management system

*Storage contract for SubscriptionManager diamond*


## State Variables
### STORAGE_SLOT

```solidity
bytes32 internal constant STORAGE_SLOT = keccak256("capsign.storage.billing");
```


### SECONDS_IN_MONTH

```solidity
uint256 internal constant SECONDS_IN_MONTH = 30 days;
```


### DISCOUNT_DENOMINATOR

```solidity
uint256 internal constant DISCOUNT_DENOMINATOR = 10000;
```


### PRICING_DECIMALS

```solidity
uint8 internal constant PRICING_DECIMALS = 6;
```


## Functions
### layout

*Get the storage layout*


```solidity
function layout() internal pure returns (Layout storage l);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`l`|`Layout`|The storage layout|


### getTier

*Get a subscription tier*


```solidity
function getTier(uint8 tierLevel) internal view returns (SubscriptionTier storage tier);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tierLevel`|`uint8`|The tier level|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`SubscriptionTier`|The subscription tier|


### getSubscription

*Get a subscriber's subscription*


```solidity
function getSubscription(address subscriber) internal view returns (Subscription storage subscription);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subscriber`|`address`|The subscriber address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`subscription`|`Subscription`|The subscription data|


### getTokenConfig

*Get token configuration*


```solidity
function getTokenConfig(address token) internal view returns (TokenConfig storage tokenConfig);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The token address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tokenConfig`|`TokenConfig`|The token configuration|


### getAddressTierDiscount

*Get address-specific tier discount*


```solidity
function getAddressTierDiscount(address subscriber, uint8 tierLevel) internal view returns (uint256 discount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subscriber`|`address`|The subscriber address|
|`tierLevel`|`uint8`|The tier level|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`discount`|`uint256`|The discount in basis points|


### setAddressTierDiscount

*Set address-specific tier discount*


```solidity
function setAddressTierDiscount(address subscriber, uint8 tierLevel, uint256 discount) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subscriber`|`address`|The subscriber address|
|`tierLevel`|`uint8`|The tier level|
|`discount`|`uint256`|The discount in basis points|


## Structs
### SubscriptionTier

```solidity
struct SubscriptionTier {
    string name;
    uint256 monthlyFeeUSD;
    bool isActive;
}
```

### Subscription

```solidity
struct Subscription {
    uint8 tierLevel;
    uint256 nextPaymentDue;
    bool isActive;
    uint256 lastPaymentAmount;
    address lastPaymentToken;
}
```

### TokenConfig

```solidity
struct TokenConfig {
    bool accepted;
    uint8 decimals;
    bool isStablecoin;
}
```

### Layout

```solidity
struct Layout {
    IERC20 cmxToken;
    address swapRouter;
    address treasury;
    uint24 uniswapPoolFee;
    mapping(uint8 => SubscriptionTier) tiers;
    mapping(address => Subscription) subscribers;
    mapping(address => TokenConfig) acceptedTokens;
    mapping(address => mapping(uint8 => uint256)) addressTierDiscounts;
    uint256 cmxDiscountBasisPoints;
    bool initialized;
}
```

