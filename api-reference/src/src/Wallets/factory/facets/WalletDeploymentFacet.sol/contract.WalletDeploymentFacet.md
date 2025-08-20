# WalletDeploymentFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Wallets/factory/facets/WalletDeploymentFacet.sol)

Facet providing wallet deployment functionality for WalletFactory diamond

*Handles the creation and initialization of SmartWallet instances using FactoryBase*


## State Variables
### WALLET_DEPLOYMENT_STORAGE_SLOT

```solidity
bytes32 private constant WALLET_DEPLOYMENT_STORAGE_SLOT = keccak256("wallet.deployment.storage");
```


## Functions
### _checkWalletTypeAllowed

Check if wallet type is allowed based on config


```solidity
function _checkWalletTypeAllowed(string memory walletType, address user) internal view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`walletType`|`string`|Type of wallet to check|
|`user`|`address`|User creating the wallet|


### _checkWalletLimits

Check wallet creation limits based on config (with tier support)


```solidity
function _checkWalletLimits(address user) internal view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User creating the wallet|


### _checkAndCollectFee

Check and collect creation fee if required


```solidity
function _checkAndCollectFee(string memory walletType) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`walletType`|`string`|Type of wallet being created|


### _checkKYCRequirements

Check KYC requirements based on config


```solidity
function _checkKYCRequirements(address user) internal view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User creating the wallet|


### createWallet

Create a standard wallet using the StandardWallet template


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


### getWalletInfo

Get wallet information by ID


```solidity
function getWalletInfo(uint256 walletId) external view returns (WalletInfo memory walletInfo);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`walletId`|`uint256`|Wallet ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`walletInfo`|`WalletInfo`|Wallet information|


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


### getWalletCreationFee

Get the current creation fee for a wallet type


```solidity
function getWalletCreationFee(string memory walletType) external view returns (uint256 fee);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`walletType`|`string`|Type of wallet to check fee for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`fee`|`uint256`|Creation fee in wei|


### isWalletTypeAllowed

Check if a wallet type is currently allowed


```solidity
function isWalletTypeAllowed(string memory walletType) external view returns (bool allowed);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`walletType`|`string`|Type of wallet to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`allowed`|`bool`|True if wallet type is allowed|


### getMaxWalletsPerUser

Get the maximum wallets allowed per user


```solidity
function getMaxWalletsPerUser() external view returns (uint256 maxWallets);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`maxWallets`|`uint256`|Maximum wallets per user (0 = unlimited)|


### getMaxWalletsForUser

Get the maximum wallets allowed for a specific user (tier-aware)


```solidity
function getMaxWalletsForUser(address user) external view returns (uint256 maxWallets);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The user address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`maxWallets`|`uint256`|Maximum wallets for this user's tier (0 = unlimited)|


### isWalletTypeAllowedForUser

Check if a wallet type is allowed for a specific user (tier-aware)


```solidity
function isWalletTypeAllowedForUser(string memory walletType, address user) external view returns (bool allowed);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`walletType`|`string`|Type of wallet to check|
|`user`|`address`|The user address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`allowed`|`bool`|True if wallet type is allowed for this user|


### getWalletCreationSummaryForUser

Get wallet creation summary for a user (tier-aware)


```solidity
function getWalletCreationSummaryForUser(address user)
    external
    view
    returns (
        uint8 tier,
        uint256 maxWallets,
        uint256 currentWallets,
        string[] memory availableWalletTypes,
        bool hasActiveSubscription
    );
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The user address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tier`|`uint8`|User's subscription tier|
|`maxWallets`|`uint256`|Maximum wallets allowed for this tier|
|`currentWallets`|`uint256`|Current number of wallets owned|
|`availableWalletTypes`|`string[]`|Wallet types available for this tier|
|`hasActiveSubscription`|`bool`|Whether user has active subscription|


### deployDiamond

Deploy a diamond with wallet-specific initialization logic

*This method is called by FactoryBase._deployDiamond() to handle wallet-specific deployment*


```solidity
function deployDiamond(IDiamond.FacetCut[] memory facetCuts, bytes memory constructorArgs, bytes32 salt)
    external
    returns (address diamond);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`facetCuts`|`IDiamond.FacetCut[]`|Array of facet cuts to install|
|`constructorArgs`|`bytes`|ABI-encoded constructor arguments|
|`salt`|`bytes32`|Salt for CREATE2 deployment|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`diamond`|`address`|Address of the deployed diamond|


### _validateWalletParams

Validate wallet creation parameters


```solidity
function _validateWalletParams(string memory walletType) internal pure;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`walletType`|`string`|Wallet type|


### _prepareWalletConstructorArgs

Prepare constructor arguments for wallet deployment


```solidity
function _prepareWalletConstructorArgs(address owner, string memory walletType)
    internal
    pure
    returns (bytes memory constructorArgs);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|Wallet owner|
|`walletType`|`string`|Wallet type|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`constructorArgs`|`bytes`|ABI-encoded constructor arguments|


### _recordWalletCreation

Record wallet creation in storage


```solidity
function _recordWalletCreation(address walletAddress, address owner, string memory walletType) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`walletAddress`|`address`|Address of created wallet|
|`owner`|`address`|Wallet owner|
|`walletType`|`string`|Wallet type|


### _getWalletDeploymentStorage

Get wallet deployment storage


```solidity
function _getWalletDeploymentStorage() internal pure returns (WalletDeploymentStorage storage s);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`s`|`WalletDeploymentStorage`|Storage struct|


### _isWalletTypeAllowed

Check if a wallet type is allowed based on config


```solidity
function _isWalletTypeAllowed(string memory walletType) internal view returns (bool allowed);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`walletType`|`string`|Type of wallet to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`allowed`|`bool`|True if wallet type is allowed|


### _getAllWalletTypes

Get all wallet types supported by this factory


```solidity
function _getAllWalletTypes() internal pure returns (string[] memory walletTypes);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`walletTypes`|`string[]`|Array of wallet type names|


### _getFeeRecipient

Get the fee recipient address from factory storage


```solidity
function _getFeeRecipient() internal view returns (address feeRecipient);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`feeRecipient`|`address`|Address to receive fees|


### getAddress

Predict the address of a wallet before deployment (AA-compatible)

*This function matches the expected ABI for account abstraction compatibility*


```solidity
function getAddress(bytes[] memory owners, uint256 nonce) external view returns (address walletAddress);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owners`|`bytes[]`|Array of owner public keys/addresses in bytes format|
|`nonce`|`uint256`|Deployment nonce for uniqueness|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`walletAddress`|`address`|Predicted address where the wallet would be deployed|


### createAccount

Create a new wallet account (Account Abstraction compatible)

*This function matches the expected ABI for account abstraction compatibility*


```solidity
function createAccount(bytes[] memory owners, uint256 nonce) external payable returns (address walletAddress);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owners`|`bytes[]`|Array of owner public keys/addresses in bytes format|
|`nonce`|`uint256`|Deployment nonce for uniqueness|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`walletAddress`|`address`|Address of the newly created wallet|


### _predictWalletAddress

Internal function to predict wallet address


```solidity
function _predictWalletAddress(bytes memory constructorArgs, bytes32 salt)
    internal
    view
    returns (address walletAddress);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`constructorArgs`|`bytes`|Constructor arguments for the wallet|
|`salt`|`bytes32`|Salt for CREATE2 deployment|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`walletAddress`|`address`|Predicted address|


## Events
### WalletCreated

```solidity
event WalletCreated(address indexed walletAddress, address indexed owner, string walletType);
```

### WalletCreationFeeCollected

```solidity
event WalletCreationFeeCollected(address indexed user, uint256 fee, string walletType);
```

## Errors
### WalletDeploymentFailed

```solidity
error WalletDeploymentFailed();
```

### InvalidWalletType

```solidity
error InvalidWalletType();
```

### WalletNameEmpty

```solidity
error WalletNameEmpty();
```

### DeploymentFailed

```solidity
error DeploymentFailed();
```

### WalletTypeNotAllowed

```solidity
error WalletTypeNotAllowed(string walletType);
```

### MaxWalletsExceeded

```solidity
error MaxWalletsExceeded(uint256 current, uint256 max);
```

### InsufficientFee

```solidity
error InsufficientFee(uint256 required, uint256 provided);
```

### KYCRequired

```solidity
error KYCRequired();
```

### TierUpgradeRequired

```solidity
error TierUpgradeRequired(string feature, uint8 currentTier, uint8 requiredTier);
```

### SubscriptionRequired

```solidity
error SubscriptionRequired(string feature);
```

## Structs
### WalletDeploymentStorage

```solidity
struct WalletDeploymentStorage {
    mapping(uint256 => WalletInfo) wallets;
    uint256 walletCount;
    mapping(address => address[]) userWallets;
    mapping(address => bool) isWallet;
}
```

### WalletInfo

```solidity
struct WalletInfo {
    address walletAddress;
    address owner;
    uint256 createdAt;
    string walletType;
}
```

