# SubscriptionViewFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Billing/facets/SubscriptionViewFacet.sol)

View functionality for the SubscriptionManager diamond

*Handles read-only queries for subscription information*


## Functions
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


### _getStorage

*Get storage layout*


```solidity
function _getStorage() internal pure returns (SubscriptionManagerStorage.Layout storage);
```

