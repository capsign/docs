# BulletinBoardMarket
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Trading/BulletinBoardMarket.sol)

**Inherits:**
[BaseMarket](/src/Trading/BaseMarket.sol/abstract.BaseMarket.md)

A simpler approach: the seller lists an asset at a set price,
any buyer can come buy it at that price. No bidding, no advanced matching.


## Functions
### createListing


```solidity
function createListing(uint96 quantity, uint256 price) external override returns (bytes32);
```

### executeTrade


```solidity
function executeTrade(bytes32 listingId, uint96 tradeQuantity) external payable override;
```

### cancelListing


```solidity
function cancelListing(bytes32 listingId) external override;
```

