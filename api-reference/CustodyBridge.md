# CustodyBridge
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Trading/Settlement/CustodyBridge.sol)

A helper (bridge) contract that coordinates moving securities between a broker's net
balance in BrokerNetSettlement and a user's DRS lot in Token.
Example usage:
1) moveToDRS(broker, user, amount, costBasis, acquisitionDate)
- Subtracts `amount` from broker's netBalance in BrokerNetSettlement.
- Creates a new lot in Token for `user`.
2) moveFromDRS(user, broker, tokenId, rawQuantity)
- Invalidates or partially sells from the user's lot in Token.
- Adds `rawQuantity` back to broker's netBalance in BrokerNetSettlement.


## State Variables
### brokerNetSettlement

```solidity
BrokerNetSettlement public brokerNetSettlement;
```


### baseAsset

```solidity
IIssuedAsset public baseAsset;
```


### owner

```solidity
address public owner;
```


## Functions
### onlyOwner


```solidity
modifier onlyOwner();
```

### constructor


```solidity
constructor(address _clearingLedger, address _baseAsset);
```

### moveToDRS

*Moves `amount` securities from `broker`'s net balance into a new DRS lot for `user`.
This effectively reduces the broker's netBalance by `amount` and calls `createLot`
in `CapitalAsset`.*


```solidity
function moveToDRS(
    address broker,
    address user,
    uint256 amount,
    uint96 costBasis,
    uint64 acquisitionDate,
    string memory uri,
    bytes memory data
) external onlyOwner returns (bytes32 tokenId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`broker`|`address`|The broker address in BrokerNetSettlement.|
|`user`|`address`|The end user who will own the new DRS lot.|
|`amount`|`uint256`|The quantity to move from netBalance to the new lot.|
|`costBasis`|`uint96`|The raw cost basis per share (if relevant).|
|`acquisitionDate`|`uint64`|A timestamp or date representing the original purchase date.|
|`uri`|`string`|The URI of the lot.|
|`data`|`bytes`|Additional data associated with the lot.|


### moveFromDRS

*Moves `rawQuantity` securities from a user's DRS lot back to a broker's netBalance.
We call `sellFromToken` on the CapitalAsset to partially or fully remove from the lot,
then increment the broker's netBalance in BrokerNetSettlement.*


```solidity
function moveFromDRS(
    address user,
    address broker,
    bytes32 tokenId,
    uint96 rawQuantity,
    string memory uri,
    bytes memory data
) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The user whose DRS lot is being reduced.|
|`broker`|`address`|The broker that regains these securities in netBalance.|
|`tokenId`|`bytes32`|The user's lot ID in CapitalAsset.|
|`rawQuantity`|`uint96`|The raw quantity to remove from the lot (not the real post-split quantity).|
|`uri`|`string`|The URI of the lot.|
|`data`|`bytes`|Additional data associated with the lot.|


### _reduceBrokerBalance

*The underlying step to reduce a broker's netBalance by `amount`.
We can do this by calling brokerNetSettlement's existing functions:
- brokerNetSettlement.transfer(...) from broker to address(0) if allowed
- or brokerNetSettlement.burn(...) if broker calls it
For simplicity, we do a "batch" approach with negative deltas if the function is public.
We illustrate a direct approach here, but it might need broker's signature in a real scenario.*


```solidity
function _reduceBrokerBalance(address broker, uint256 amount) internal;
```

### _increaseBrokerBalance

*Internal function to increase a broker's netBalance in BrokerNetSettlement.
Utilizes the BrokerNetSettlement's `createLot` function to add securities.*


```solidity
function _increaseBrokerBalance(address broker, uint256 amount) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`broker`|`address`|The broker whose netBalance is to be increased.|
|`amount`|`uint256`|The amount to increase.|


### setClearingLedger

*Allows the owner to update the BrokerNetSettlement address.*


```solidity
function setClearingLedger(address _clearingLedger) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_clearingLedger`|`address`|The new BrokerNetSettlement address.|


### setAssetRegistry

*Allows the owner to update the Token address.*


```solidity
function setAssetRegistry(address _baseAsset) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_baseAsset`|`address`|The new Token address.|


### setOwner

*(Optional) convenience function for advanced bridging, e.g. user -> user direct or
multiple brokers in one go, etc. Kept minimal in this example.*


```solidity
function setOwner(address newOwner) external onlyOwner;
```

## Events
### MovedToDRS

```solidity
event MovedToDRS(address indexed broker, address indexed user, uint256 amount, bytes32 tokenId, string uri, bytes data);
```

### MovedFromDRS

```solidity
event MovedFromDRS(
    address indexed user, address indexed broker, bytes32 tokenId, uint256 amount, string uri, bytes data
);
```

