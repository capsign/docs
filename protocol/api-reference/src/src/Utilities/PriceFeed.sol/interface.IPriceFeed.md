# IPriceFeed
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Utilities/PriceFeed.sol)

Interface for fetching current prices of securities.


## Functions
### getPrice

*Returns the current price of a given security.*


```solidity
function getPrice(bytes32 securityId) external view returns (uint256 price);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`securityId`|`bytes32`|The identifier of the security.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`price`|`uint256`|The current price.|


