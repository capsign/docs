# DelegationFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Wallets/wallet/facets/DelegationFacet.sol)

**Inherits:**
[MultiOwnable](/src/Wallets/wallet/MultiOwnable.sol/contract.MultiOwnable.md)

*Handles delegation of transaction execution to other wallets with proper authorization*


## Functions
### onlyOwner


```solidity
modifier onlyOwner() override;
```

### onlyActiveDelegator


```solidity
modifier onlyActiveDelegator();
```

### configureDelegation

*Configure delegation for another wallet*


```solidity
function configureDelegation(
    address delegatedWallet,
    uint256 maxValue,
    uint256 dailyLimit,
    bytes4[] calldata allowedSelectors
) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`delegatedWallet`|`address`|The wallet that can execute transactions on behalf of this wallet|
|`maxValue`|`uint256`|Maximum value per transaction|
|`dailyLimit`|`uint256`|Maximum daily spending limit|
|`allowedSelectors`|`bytes4[]`|Array of function selectors that can be delegated|


### revokeDelegation

*Revoke delegation for a wallet*


```solidity
function revokeDelegation(address delegatedWallet) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`delegatedWallet`|`address`|The wallet to revoke delegation from|


### setFunctionDelegation

*Allow/disallow a specific function for delegation*


```solidity
function setFunctionDelegation(address delegatedWallet, bytes4 selector, bool allowed) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`delegatedWallet`|`address`|The delegated wallet|
|`selector`|`bytes4`|Function selector|
|`allowed`|`bool`|Whether the function is allowed|


### executeDelegated

*Execute a transaction as a delegated wallet*


```solidity
function executeDelegated(address target, uint256 value, bytes calldata data, bytes calldata ownerSignature)
    external
    onlyActiveDelegator
    returns (bool success, bytes memory result);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`target`|`address`|Target contract address|
|`value`|`uint256`|ETH value to send|
|`data`|`bytes`|Transaction data|
|`ownerSignature`|`bytes`|Signature from an owner of the delegating wallet|


### executeDelegatedBatch

*Execute multiple transactions as a delegated wallet*


```solidity
function executeDelegatedBatch(
    address[] calldata targets,
    uint256[] calldata values,
    bytes[] calldata datas,
    bytes calldata ownerSignature
) external onlyActiveDelegator;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`targets`|`address[]`|Array of target addresses|
|`values`|`uint256[]`|Array of ETH values|
|`datas`|`bytes[]`|Array of transaction data|
|`ownerSignature`|`bytes`|Signature from an owner of the delegating wallet|


### getDelegationConfig

*Get delegation configuration for a wallet*


```solidity
function getDelegationConfig(address delegatedWallet)
    external
    view
    returns (uint256 maxValue, uint256 dailyLimit, uint256 dailySpent, uint256 lastResetTime, bool active);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`delegatedWallet`|`address`|The delegated wallet|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`maxValue`|`uint256`|Maximum value per transaction|
|`dailyLimit`|`uint256`|Daily spending limit|
|`dailySpent`|`uint256`|Amount spent today|
|`lastResetTime`|`uint256`|Last time daily limit was reset|
|`active`|`bool`|Whether delegation is active|


### isFunctionDelegated

*Check if a function is delegated to a wallet*


```solidity
function isFunctionDelegated(address delegatedWallet, bytes4 selector) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`delegatedWallet`|`address`|The delegated wallet|
|`selector`|`bytes4`|Function selector|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|Whether the function is delegated|


### getDelegatedWallets

*Get all delegated wallets*


```solidity
function getDelegatedWallets() external view returns (address[] memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Array of delegated wallet addresses|


### updateDailyLimit

*Update daily limit for a delegated wallet*


```solidity
function updateDailyLimit(address delegatedWallet, uint256 newDailyLimit) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`delegatedWallet`|`address`|The delegated wallet|
|`newDailyLimit`|`uint256`|New daily limit|


### resetDailySpending

*Reset daily spending for a delegated wallet (emergency function)*


```solidity
function resetDailySpending(address delegatedWallet) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`delegatedWallet`|`address`|The delegated wallet|


### canDelegate

*Check if delegation is allowed for a specific transaction*


```solidity
function canDelegate(address delegatedWallet, bytes4 selector, uint256 value) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`delegatedWallet`|`address`|The delegated wallet|
|`selector`|`bytes4`|Function selector|
|`value`|`uint256`|Transaction value|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|Whether delegation is allowed|


### _verifyOwnerSignature

*Verify owner signature for delegated transaction*


```solidity
function _verifyOwnerSignature(bytes32 hash, bytes calldata signature) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`hash`|`bytes32`|Transaction hash|
|`signature`|`bytes`|Owner signature|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|Whether signature is valid|


## Events
### DelegationConfigured

```solidity
event DelegationConfigured(address indexed delegatedWallet, uint256 maxValue, uint256 dailyLimit);
```

### DelegationRevoked

```solidity
event DelegationRevoked(address indexed delegatedWallet);
```

### FunctionDelegated

```solidity
event FunctionDelegated(address indexed delegatedWallet, bytes4 indexed selector, bool allowed);
```

### DelegatedTransactionExecuted

```solidity
event DelegatedTransactionExecuted(
    address indexed delegatedWallet, address indexed target, uint256 value, bytes4 selector, bool success
);
```

### DailyLimitReset

```solidity
event DailyLimitReset(address indexed delegatedWallet, uint256 newLimit);
```

## Errors
### NotAuthorizedDelegator

```solidity
error NotAuthorizedDelegator();
```

### DelegationNotActive

```solidity
error DelegationNotActive();
```

### FunctionNotDelegated

```solidity
error FunctionNotDelegated();
```

### ValueExceedsLimit

```solidity
error ValueExceedsLimit();
```

### DailyLimitExceeded

```solidity
error DailyLimitExceeded();
```

### InvalidDelegationConfig

```solidity
error InvalidDelegationConfig();
```

