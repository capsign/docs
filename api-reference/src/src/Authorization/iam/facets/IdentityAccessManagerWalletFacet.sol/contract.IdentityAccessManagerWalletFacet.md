# IdentityAccessManagerWalletFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Authorization/iam/facets/IdentityAccessManagerWalletFacet.sol)

Allows EAMs to deploy and manage wallets for their entities

*Facet for wallet deployment and management through IAM*


## State Variables
### _walletFactory

```solidity
mapping(address => address) private _walletFactory;
```


### _deployedWallets

```solidity
mapping(address => address[]) private _deployedWallets;
```


### _walletExists

```solidity
mapping(address => mapping(address => bool)) private _walletExists;
```


## Functions
### constructor

*Constructor - no dependencies to avoid circular dependency during dEAMond construction*


```solidity
constructor();
```

### setWalletFactory

*Set the wallet factory for this IAM*


```solidity
function setWalletFactory(address walletFactory) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`walletFactory`|`address`|The wallet factory address|


### deployWallet

*Deploy a standard wallet through the IAM*


```solidity
function deployWallet(string calldata name, string calldata walletType) external returns (address wallet);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`name`|`string`|The name for the wallet|
|`walletType`|`string`|The type of wallet to deploy|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`wallet`|`address`|The address of the deployed wallet|


### deployGovernanceWallet

*Deploy a governance wallet through the IAM*


```solidity
function deployGovernanceWallet(string calldata governanceType) external returns (address wallet);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`governanceType`|`string`|The type of governance (e.g., "SimpleApproval", "MultiSig", "FullGovernance")|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`wallet`|`address`|The address of the deployed governance wallet|


### getDeployedWallets

*Get all wallets deployed by this IAM*


```solidity
function getDeployedWallets() external view returns (address[] memory wallets);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`wallets`|`address[]`|Array of wallet addresses|


### getWalletCount

*Get the number of wallets deployed by this IAM*


```solidity
function getWalletCount() external view returns (uint256 count);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`count`|`uint256`|Number of deployed wallets|


### isWalletDeployed

*Check if a wallet was deployed by this IAM*


```solidity
function isWalletDeployed(address wallet) external view returns (bool exists);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`wallet`|`address`|The wallet address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`exists`|`bool`|True if the wallet was deployed by this IAM|


### getWalletFactory

*Get the current wallet factory for this IAM*


```solidity
function getWalletFactory() external view returns (address factory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`factory`|`address`|The wallet factory address|


### _ensureIdentityAccess

*Ensure only the entity (IAM owner) can call wallet functions*


```solidity
function _ensureIdentityAccess() internal view;
```

### initializeWalletFactory

*Initialize wallet factory for a new IAM*


```solidity
function initializeWalletFactory(address IAM, address walletFactory) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`IAM`|`address`|The IAM address|
|`walletFactory`|`address`|The wallet factory address|


## Events
### WalletDeployed

```solidity
event WalletDeployed(address indexed IAM, address indexed wallet, string walletType, address indexed creator);
```

### GovernanceWalletDeployed

```solidity
event GovernanceWalletDeployed(
    address indexed IAM, address indexed wallet, string governanceType, address indexed creator
);
```

### WalletFactoryUpdated

```solidity
event WalletFactoryUpdated(address indexed oldFactory, address indexed newFactory);
```

## Errors
### WalletFactoryNotSet

```solidity
error WalletFactoryNotSet();
```

### WalletDeploymentFailed

```solidity
error WalletDeploymentFailed();
```

### OnlyEntityCanDeploy

```solidity
error OnlyEntityCanDeploy();
```

