# BrokerNetSettlement
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Trading/Settlement/BrokerNetSettlement.sol)

Tracks broker-level net balances for a single security, allowing
a clearing operator to batch-settle trades and apply global adjustments
(e.g., stock splits) by physically updating all broker balances.
- No DRS logic here; end users are off-chain at each broker.
- No cost basis tracking for brokers.
- Global splits are done by enumerating broker addresses in one transaction.


## State Variables
### owner

```solidity
address public owner;
```


### clearingOperator

```solidity
address public clearingOperator;
```


### netBalance
*"netBalance" indicates how many shares an address holds in a fungible, nettable form.*


```solidity
mapping(address => uint256) public netBalance;
```


### totalSupply

```solidity
uint256 public totalSupply;
```


## Functions
### onlyOwner


```solidity
modifier onlyOwner();
```

### onlyClearingOperator


```solidity
modifier onlyClearingOperator();
```

### constructor


```solidity
constructor();
```

### createLot


```solidity
function createLot(address broker, uint256 amount) external onlyOwner;
```

### burn


```solidity
function burn(uint256 amount) external;
```

### transfer

*A simple net-balance transfer, akin to ERC-20 transfer.
Does NOT touch DRS shares.*


```solidity
function transfer(address to, uint256 amount) external;
```

### netSettlementBatch

*The clearing operator can apply net deltas to multiple accounts.
Example usage: after end-of-day, each address's net buy or net sell
is aggregated. If the user net-buys 100 shares, delta = +100.
If net-sells 50 shares, delta = -50, etc.
Requirements:
- Address must have enough netBalance if delta is negative,
or enough drsBalance if you want to allow forced adjustments from DRS (not shown here).*


```solidity
function netSettlementBatch(address[] calldata accounts, int256[] calldata deltas) external onlyClearingOperator;
```

### applyGlobalSplit

*Physically updates each broker's netBalance for a declared split ratio.
E.g., for a 2-for-1 split, splitNum=2, splitDen=1.
For a 1-for-2 reverse split, splitNum=1, splitDen=2.*


```solidity
function applyGlobalSplit(address[] calldata brokers, uint96 splitNum, uint96 splitDen) external onlyClearingOperator;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`brokers`|`address[]`|The array of broker addresses to adjust.|
|`splitNum`|`uint96`|Numerator of the split ratio.|
|`splitDen`|`uint96`|Denominator of the split ratio.|


### applyStockDividend

*Grants each broker new shares proportional to their existing balance.
e.g., a 10% stock dividend => divNum=10, divDen=100.
newShares = oldBal * divNum / divDen.
totalSupply is increased accordingly.*


```solidity
function applyStockDividend(address[] calldata brokers, uint96 divNum, uint96 divDen) external onlyClearingOperator;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`brokers`|`address[]`|The broker addresses to update.|
|`divNum`|`uint96`|Numerator of dividend ratio (e.g. 10 for 10%).|
|`divDen`|`uint96`|Denominator of dividend ratio (e.g. 100).|


### setClearingOperator


```solidity
function setClearingOperator(address _op) external onlyOwner;
```

## Events
### Transfer

```solidity
event Transfer(address indexed from, address indexed to, uint256 amount);
```

### NetSettlementPerformed

```solidity
event NetSettlementPerformed(address indexed operator, address[] brokers, int256[] deltas);
```

### GlobalSplitApplied

```solidity
event GlobalSplitApplied(uint96 splitNum, uint96 splitDen, address[] brokers);
```

### StockDividendApplied

```solidity
event StockDividendApplied(uint96 divNum, uint96 divDen, address[] brokers);
```

