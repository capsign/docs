# WalletFactoryStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Wallets/factory/storage/WalletFactoryStorage.sol)

Storage library for wallet factory configuration using diamond storage pattern

*Provides storage for wallet factory settings like fees, limits, and permissions*


## State Variables
### WALLET_FACTORY_STORAGE_SLOT

```solidity
bytes32 private constant WALLET_FACTORY_STORAGE_SLOT = keccak256("capsign.storage.wallet_factory");
```


## Functions
### layout

Get the storage layout for wallet factory configuration


```solidity
function layout() internal pure returns (Layout storage l);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`l`|`Layout`|The storage layout|


### initialize

Initialize wallet factory storage


```solidity
function initialize(address feeRecipient, uint256 creationFee, uint256 maxWalletsPerUser) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`feeRecipient`|`address`|Initial fee recipient|
|`creationFee`|`uint256`|Initial creation fee|
|`maxWalletsPerUser`|`uint256`|Initial max wallets per user|


### isInitialized

Check if wallet factory is initialized


```solidity
function isInitialized() internal view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if initialized|


### incrementWalletsCreated

Increment total wallets created counter


```solidity
function incrementWalletsCreated() internal;
```

### getTotalWalletsCreated

Get total wallets created


```solidity
function getTotalWalletsCreated() internal view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Total number of wallets created|


### setWalletTypeAllowed

Set wallet type permission


```solidity
function setWalletTypeAllowed(string memory walletType, bool allowed) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`walletType`|`string`|The wallet type|
|`allowed`|`bool`|Whether the type is allowed|


### isWalletTypeAllowed

Check if wallet type is allowed


```solidity
function isWalletTypeAllowed(string memory walletType) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`walletType`|`string`|The wallet type|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if allowed|


### setFeeRecipient

Set fee recipient address


```solidity
function setFeeRecipient(address feeRecipient) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`feeRecipient`|`address`|New fee recipient address|


### getFeeRecipient

Get fee recipient address


```solidity
function getFeeRecipient() internal view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|Fee recipient address|


## Structs
### Layout

```solidity
struct Layout {
    uint256 creationFee;
    address feeRecipient;
    uint256 maxWalletsPerUser;
    uint256 maxWalletsTotal;
    mapping(string => bool) allowedWalletTypes;
    uint256 totalWalletsCreated;
    bool initialized;
    mapping(address => bool) supportedCMXPaymasters;
    address[] cmxPaymasterList;
    uint256[50] __gap;
}
```

