# AuctionMarket
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Trading/AuctionMarket.sol)

**Inherits:**
[BaseMarket](/src/Trading/BaseMarket.sol/abstract.BaseMarket.md)

1 seller listing a block of shares with multiple potential bidders,
can run an English Auction, Dutch Auction, sealed-bid, etc.


## Functions
### createListing


```solidity
function createListing(uint96 quantity, uint256 startPrice) external override returns (bytes32);
```

### executeTrade


```solidity
function executeTrade(bytes32 listingId, uint96 tradeQuantity) external payable override;
```

### cancelListing


```solidity
function cancelListing(bytes32 listingId) external override;
```

