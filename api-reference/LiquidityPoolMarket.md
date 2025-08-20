# LiquidityPoolMarket
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Trading/LiquidityPoolMarket.sol)

**Inherits:**
[BaseMarket](/src/Trading/BaseMarket.sol/abstract.BaseMarket.md)

Implements an Automated Market Maker (AMM) style liquidity pool for securities.


## State Variables
### paymentCurrency

```solidity
IERC20 public paymentCurrency;
```


### shareClass

```solidity
IShareClass public shareClass;
```


### reserveShares

```solidity
uint256 public reserveShares;
```


### reservePayments

```solidity
uint256 public reservePayments;
```


## Functions
### constructor


```solidity
constructor(address _paymentCurrency, address _shareClass);
```

### addLiquidity

*Adds liquidity to the pool.*


```solidity
function addLiquidity(uint256 shares, uint256 payments, string memory uri, bytes memory data) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`shares`|`uint256`|The number of shares to add.|
|`payments`|`uint256`|The amount of payment tokens to add.|
|`uri`|`string`||
|`data`|`bytes`||


### removeLiquidity

*Removes liquidity from the pool.*


```solidity
function removeLiquidity(uint256 shares, uint256 payments, string memory uri, bytes memory data) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`shares`|`uint256`|The number of shares to remove.|
|`payments`|`uint256`|The amount of payment tokens to remove.|
|`uri`|`string`||
|`data`|`bytes`||


### executeTrade

*Executes a trade by swapping payment tokens for shares.*


```solidity
function executeTrade(uint256 sharesBought, string memory uri, bytes memory data) external payable;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`sharesBought`|`uint256`|The number of shares the buyer wants to purchase.|
|`uri`|`string`||
|`data`|`bytes`||


### createListing


```solidity
function createListing(uint96, uint256) external pure override returns (bytes32);
```

### executeTrade


```solidity
function executeTrade(bytes32, uint96) external payable override;
```

### cancelListing


```solidity
function cancelListing(bytes32) external pure override;
```

## Events
### LiquidityAdded

```solidity
event LiquidityAdded(address indexed provider, uint256 shares, uint256 payments);
```

### LiquidityRemoved

```solidity
event LiquidityRemoved(address indexed provider, uint256 shares, uint256 payments);
```

### TradeExecuted

```solidity
event TradeExecuted(address indexed buyer, uint256 sharesBought, uint256 paymentsPaid);
```

