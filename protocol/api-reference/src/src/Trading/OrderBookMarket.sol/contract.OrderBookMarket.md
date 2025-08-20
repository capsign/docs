# OrderBookMarket
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Trading/OrderBookMarket.sol)

**Inherits:**
[BaseMarket](/src/Trading/BaseMarket.sol/abstract.BaseMarket.md)

A more advanced market with multiple asks and bids, matching them on
price-time priority or some other logic. Here we integrate with:
- ListingRegistry for multi-lot listings
- BrokerNetSettlement for broker-based settlement
- DirectSettlement for on-chain direct settlement


## State Variables
### listingRegistry

```solidity
ListingRegistry public listingRegistry;
```


### brokerNetSettlement

```solidity
BrokerNetSettlement public brokerNetSettlement;
```


### directSettlement

```solidity
DirectSettlement public directSettlement;
```


### BROKER_CHECK_CONTRACT

```solidity
address constant BROKER_CHECK_CONTRACT = 0x000000000000000000000000000000000000b0b0;
```


### orderBookMapping

```solidity
mapping(bytes32 => uint96) public orderBookMapping;
```


## Functions
### constructor


```solidity
constructor(address _listingRegistry, address _brokerNetSettlement, address _directSettlement);
```

### createListing

*Creates a listing in the order book. For demonstration, we just create
a single-lot array or a multi-lot array. Returns listingId.*


```solidity
function createListing(uint96 quantity, uint256 price) external override returns (bytes32);
```

### executeTrade

*Executes (fills) a trade against an existing listing.
If there's more shares requested than available, it reverts.
If fully filled, the listing is removed from the registry.
Splits the logic between broker settlement or direct settlement.*


```solidity
function executeTrade(bytes32 listingId, uint96 tradeQuantity) external payable override;
```

### cancelListing

*Cancels a listing if the seller no longer wants to sell or if partially filled.*


```solidity
function cancelListing(bytes32 listingId) external override;
```

### _settleBrokerNet

*Settlement logic for broker-based trades that uses BrokerNetSettlement.
- If both buyer and seller are brokers, we simply update net balances.
- If only one side is broker, you could optionally do partial direct settlement,
but for simplicity, we'll treat either side as a broker => net settlement approach.*


```solidity
function _settleBrokerNet(address buyer, address seller, uint96 quantity, uint256) internal;
```

### isBroker

*Stub function to call a "fake" broker-check contract.
Returns true if address is recognized as a broker.*


```solidity
function isBroker(address account) internal view returns (bool);
```

