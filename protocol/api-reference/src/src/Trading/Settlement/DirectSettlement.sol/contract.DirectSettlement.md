# DirectSettlement
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Trading/Settlement/DirectSettlement.sol)

Manages the settlement of trades, transferring shares and payments between parties.


## State Variables
### paymentCurrency

```solidity
IERC20 public paymentCurrency;
```


### shareClass

```solidity
IShareClass public shareClass;
```


## Functions
### constructor


```solidity
constructor(address _paymentCurrency, address _shareClass);
```

### settleTrade

*Settles a trade by transferring payment from buyer to seller and shares from seller to buyer.*


```solidity
function settleTrade(
    bytes32 listingId,
    address buyer,
    address seller,
    uint96 quantity,
    uint256 totalPrice,
    string memory uri,
    bytes memory data
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`listingId`|`bytes32`|The ID of the listing being settled.|
|`buyer`|`address`|The address of the buyer.|
|`seller`|`address`|The address of the seller.|
|`quantity`|`uint96`|The number of shares being transferred.|
|`totalPrice`|`uint256`|The total price (quantity * price per share).|
|`uri`|`string`|The URI of the lot.|
|`data`|`bytes`|Additional data associated with the lot.|


## Events
### TradeSettled

```solidity
event TradeSettled(
    bytes32 indexed listingId, address indexed buyer, address indexed seller, uint96 quantity, uint256 totalPrice
);
```

