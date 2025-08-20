# BaseMarket
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Trading/BaseMarket.sol)

Abstract contract defining the core interface and events for
matching trades or listing assets in a secondary market of tokenized securities.


## Functions
### createListing

*Lists a certain quantity of shares (tokens) for sale or sets up a new listing.
Implementation details differ for OrderBook vs Auction vs BulletinBoard, etc.*


```solidity
function createListing(uint96 quantity, uint256 priceOrStart) external virtual returns (bytes32);
```

### executeTrade

*Executes or matches a trade against an existing listing.
Implementation differs in each derived contract.*


```solidity
function executeTrade(bytes32 listingId, uint96 tradeQuantity) external payable virtual;
```

### cancelListing

*Cancels a listing if the seller no longer wants to sell, or the market ends.*


```solidity
function cancelListing(bytes32 listingId) external virtual;
```

## Events
### ListingCreated

```solidity
event ListingCreated(bytes32 indexed listingId, address indexed lister, uint96 quantity);
```

### TradeExecuted

```solidity
event TradeExecuted(bytes32 indexed listingId, address indexed buyer, uint96 quantity, uint256 price);
```

