# IWalletFactory
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Wallets/factory/interfaces/IWalletFactory.sol)

**Inherits:**
[IFactory](/src/Diamonds/factory/IFactory.sol/interface.IFactory.md)

Interface for WalletFactory diamond extending IFactory

*Combines FactoryBase functionality with wallet-specific functions and unified access control*


## Functions
### createWallet

Create a wallet with specified parameters


```solidity
function createWallet(string memory walletType) external payable returns (address walletAddress);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`walletType`|`string`|Type of wallet to create (string-based)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`walletAddress`|`address`|Address of the created wallet|


### createWalletWithTemplate

Create a wallet using a specific template


```solidity
function createWalletWithTemplate(string memory templateName, string memory walletType)
    external
    payable
    returns (address walletAddress);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`templateName`|`string`|Name of the template to use|
|`walletType`|`string`|Type of wallet to create (string-based)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`walletAddress`|`address`|Address of the created wallet|


### createWalletWithCustomFacets

Create a wallet with custom facets


```solidity
function createWalletWithCustomFacets(string[] memory facetNames, string memory walletType)
    external
    payable
    returns (address walletAddress);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`facetNames`|`string[]`|Array of facet names to include|
|`walletType`|`string`|Type of wallet to create (string-based)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`walletAddress`|`address`|Address of the created wallet|


### createIndividualWallet

Create an individual wallet (convenience function)


```solidity
function createIndividualWallet() external payable returns (address walletAddress);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`walletAddress`|`address`|Address of the created wallet|


### createEntityWallet

Create an entity wallet (convenience function)


```solidity
function createEntityWallet() external payable returns (address walletAddress);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`walletAddress`|`address`|Address of the created wallet|


### createJointWallet

Create a joint wallet (convenience function)


```solidity
function createJointWallet() external payable returns (address walletAddress);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`walletAddress`|`address`|Address of the created wallet|


### getTotalWalletCount

Get total number of wallets created


```solidity
function getTotalWalletCount() external view returns (uint256);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Total wallet count|


### getUserWallets

Get wallet addresses for a specific user


```solidity
function getUserWallets(address user) external view returns (address[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Array of wallet addresses|


### getUserWallets

Get caller's wallet addresses


```solidity
function getUserWallets() external view returns (address[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Array of wallet addresses|


### getWalletInfo

Get wallet information by index


```solidity
function getWalletInfo(uint256 index)
    external
    view
    returns (address walletAddress, address owner, uint256 createdAt, string memory name, string memory walletType);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`index`|`uint256`|Wallet index|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`walletAddress`|`address`|The address of the wallet|
|`owner`|`address`|The owner of the wallet|
|`createdAt`|`uint256`|The timestamp when the wallet was created|
|`name`|`string`|The name of the wallet|
|`walletType`|`string`|The type of the wallet|


### isWalletFromFactory

Check if address is a wallet from this factory


```solidity
function isWalletFromFactory(address wallet) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`wallet`|`address`|Address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if wallet is from this factory|


### isWallet

Check if an address is a deployed wallet


```solidity
function isWallet(address walletAddress) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`walletAddress`|`address`|Address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if address is a deployed wallet|


## Events
### WalletCreated

```solidity
event WalletCreated(address indexed walletAddress, address indexed owner, string walletType);
```

