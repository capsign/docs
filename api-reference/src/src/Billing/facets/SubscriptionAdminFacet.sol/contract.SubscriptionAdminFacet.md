# SubscriptionAdminFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Billing/facets/SubscriptionAdminFacet.sol)

**Inherits:**
[AccessManaged](/src/Authorization/AccessManaged.sol/abstract.AccessManaged.md)

Administrative functionality for the SubscriptionManager diamond

*Handles tier management, token configuration, and discount settings*


## Functions
### constructor

*Constructor - facet inherits AccessManaged like other working facets*


```solidity
constructor(address _globalAccessManager) AccessManaged(_globalAccessManager);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_globalAccessManager`|`address`|Address of the GlobalAccessManager|


### onlyProtocolAdmin

Modifier to check if caller has PROTOCOL_ADMIN_ROLE


```solidity
modifier onlyProtocolAdmin();
```

### initialize

Initialize the subscription manager


```solidity
function initialize(address _cmxToken, address _swapRouter, address _treasury, address _initialOwner)
    external
    onlyProtocolAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_cmxToken`|`address`|Address of the CMX token|
|`_swapRouter`|`address`|Address of the Uniswap V3 SwapRouter|
|`_treasury`|`address`|Address to receive payments|
|`_initialOwner`|`address`|Address that will become the owner of the contract (unused in current implementation)|


### setTier

Creates or updates a subscription tier


```solidity
function setTier(uint8 tierLevel, string calldata name, uint256 monthlyFeeUSD, bool isActive)
    external
    onlyProtocolAdmin;
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
function addAcceptedToken(address token, uint8 decimals, bool isStablecoin) external onlyProtocolAdmin;
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
function removeAcceptedToken(address token) external onlyProtocolAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|The token address to remove|


### setCMXDiscount

Sets the CMX discount percentage


```solidity
function setCMXDiscount(uint256 discountBasisPoints) external onlyProtocolAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`discountBasisPoints`|`uint256`|Discount in basis points (e.g., 2000 = 20%)|


### setAddressTierDiscount

Sets a customer-specific discount for a specific tier (only applies to CMX payments)


```solidity
function setAddressTierDiscount(address subscriber, uint8 tierLevel, uint256 discountBasisPoints)
    external
    onlyProtocolAdmin;
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
function adminSetSubscription(address subscriber, uint8 tierLevel, uint256 durationInSeconds)
    external
    onlyProtocolAdmin;
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
function adminCancelSubscription(address subscriber) external onlyProtocolAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subscriber`|`address`|The subscriber address|


### setTreasury

Updates the treasury address


```solidity
function setTreasury(address newTreasury) external onlyProtocolAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newTreasury`|`address`|New treasury address|


### setUniswapPoolFee

Sets the Uniswap pool fee for token swaps (for future ETH support)


```solidity
function setUniswapPoolFee(uint24 newFee) external onlyProtocolAdmin;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newFee`|`uint24`|New fee tier (e.g., 3000 for 0.3%)|


### _setTier

*Internal function to set tier*


```solidity
function _setTier(uint8 tierLevel, string memory name, uint256 monthlyFeeUSD, bool isActive) internal;
```

### _getStorage

*Get storage layout*


```solidity
function _getStorage() internal pure returns (SubscriptionManagerStorage.Layout storage);
```

### _checkProtocolAdmin

Check if caller has PROTOCOL_ADMIN_ROLE via GAM


```solidity
function _checkProtocolAdmin() internal view;
```

### _getAccessManager

Get the access manager from the inherited AccessManaged

*Uses the inherited authority() function directly like other working facets*


```solidity
function _getAccessManager() internal view returns (address);
```

## Events
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

## Errors
### InvalidTierLevel

```solidity
error InvalidTierLevel();
```

### InvalidTokenAddress

```solidity
error InvalidTokenAddress();
```

### InvalidDecimals

```solidity
error InvalidDecimals();
```

### InvalidDiscount

```solidity
error InvalidDiscount();
```

### InvalidTreasuryAddress

```solidity
error InvalidTreasuryAddress();
```

### CannotRemoveCMXToken

```solidity
error CannotRemoveCMXToken();
```

### Unauthorized

```solidity
error Unauthorized();
```

### NoAccessManager

```solidity
error NoAccessManager();
```

