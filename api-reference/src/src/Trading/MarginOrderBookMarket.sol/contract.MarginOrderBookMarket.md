# MarginOrderBookMarket
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Trading/MarginOrderBookMarket.sol)

**Inherits:**
[OrderBookMarket](/src/Trading/OrderBookMarket.sol/contract.OrderBookMarket.md)

Extends OrderBookMarket to support margin trading.


## State Variables
### shareClass

```solidity
IShareClass public shareClass;
```


### marginAccounts

```solidity
mapping(address => MarginAccount) public marginAccounts;
```


## Functions
### constructor


```solidity
constructor(address _listingRegistry, address _brokerNetSettlement, address _directSettlement, address _shareClass)
    OrderBookMarket(_listingRegistry, _brokerNetSettlement, _directSettlement);
```

### createMarginAccount

*Creates a margin account for a user.*


```solidity
function createMarginAccount(address user, address collateral, uint256 collateralFactor) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address of the user.|
|`collateral`|`address`|The address of the collateral contract.|
|`collateralFactor`|`uint256`|The collateral factor (e.g., 150 for 150%).|


### executeMarginTrade

*Example function to execute a trade with margin.*


```solidity
function executeMarginTrade(bytes32 listingId, uint96 tradeQuantity) external payable;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`listingId`|`bytes32`|The ID of the listing.|
|`tradeQuantity`|`uint96`|The quantity to trade.|


## Events
### MarginAccountCreated

```solidity
event MarginAccountCreated(address indexed user, address marginAccount);
```

