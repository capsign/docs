# SimplePriceFeed
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Utilities/PriceFeed.sol)

**Inherits:**
[IPriceFeed](/src/Utilities/PriceFeed.sol/interface.IPriceFeed.md)

A simplistic implementation of IPriceFeed for demonstration purposes.


## State Variables
### prices

```solidity
mapping(bytes32 => uint256) private prices;
```


## Functions
### setPrice

*Sets the price for a security.*


```solidity
function setPrice(bytes32 securityId, uint256 price) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`securityId`|`bytes32`|The identifier of the security.|
|`price`|`uint256`|The new price.|


### getPrice

*Fetches the current price of a security.*


```solidity
function getPrice(bytes32 securityId) external view override returns (uint256 price);
```

## Events
### PriceUpdated

```solidity
event PriceUpdated(bytes32 indexed securityId, uint256 newPrice);
```

