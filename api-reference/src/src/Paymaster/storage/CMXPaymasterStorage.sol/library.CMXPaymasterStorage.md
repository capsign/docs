# CMXPaymasterStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Paymaster/storage/CMXPaymasterStorage.sol)

Diamond storage library for CMX paymaster

*Provides storage for CMX token-based fee payments, exchange rates, and factory management*


## State Variables
### CMX_PAYMASTER_STORAGE_SLOT

```solidity
bytes32 private constant CMX_PAYMASTER_STORAGE_SLOT = keccak256("capsign.storage.paymaster");
```


## Functions
### layout

Get the storage layout for CMX paymaster


```solidity
function layout() internal pure returns (Layout storage l);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`l`|`Layout`|The storage layout|


### initialize

Initialize CMX paymaster storage


```solidity
function initialize(address cmxToken, address entryPoint, address protocolTreasury, uint256 initialExchangeRate)
    internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`cmxToken`|`address`|CMX token contract address|
|`entryPoint`|`address`|EntryPoint contract address|
|`protocolTreasury`|`address`|Protocol treasury address|
|`initialExchangeRate`|`uint256`|Initial CMX/ETH exchange rate|


### isFactorySupported

Check if factory is supported


```solidity
function isFactorySupported(address factory) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`factory`|`address`|Factory address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|supported Whether the factory is supported|


### getFactoryFeeMultiplier

Get factory fee multiplier


```solidity
function getFactoryFeeMultiplier(address factory) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`factory`|`address`|Factory address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|multiplier Fee multiplier (scaled by 1e18)|


### addSupportedFactory

Add supported factory


```solidity
function addSupportedFactory(address factory, uint256 feeMultiplier) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`factory`|`address`|Factory address|
|`feeMultiplier`|`uint256`|Fee multiplier (scaled by 1e18)|


### removeSupportedFactory

Remove supported factory


```solidity
function removeSupportedFactory(address factory) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`factory`|`address`|Factory address|


### updateUserStats

Update user statistics


```solidity
function updateUserStats(address user, uint256 cmxUsed, uint256 ethEquivalent) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User address|
|`cmxUsed`|`uint256`|CMX amount used|
|`ethEquivalent`|`uint256`|ETH equivalent amount|


### checkRateLimits

Check and update rate limits


```solidity
function checkRateLimits(address user) internal returns (bool allowed);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`allowed`|`bool`|Whether the operation is allowed|


### calculateCMXAmount

Calculate CMX amount needed for ETH amount


```solidity
function calculateCMXAmount(uint256 ethAmount) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ethAmount`|`uint256`|ETH amount in wei|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|cmxAmount Required CMX amount|


### calculateETHEquivalent

Calculate ETH equivalent for CMX amount


```solidity
function calculateETHEquivalent(uint256 cmxAmount) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`cmxAmount`|`uint256`|CMX amount|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|ethAmount Equivalent ETH amount in wei|


### isPaymasterManager

Check if address is authorized paymaster manager


```solidity
function isPaymasterManager(address account) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|Address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|authorized Whether the address is authorized|


### isTreasuryManager

Check if address is authorized treasury manager


```solidity
function isTreasuryManager(address account) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|Address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|authorized Whether the address is authorized|


### isRateUpdater

Check if address is authorized rate updater


```solidity
function isRateUpdater(address account) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|Address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|authorized Whether the address is authorized|


## Structs
### FactoryConfig

```solidity
struct FactoryConfig {
    bool supported;
    uint256 feeMultiplier;
}
```

### UserStats

```solidity
struct UserStats {
    uint256 operationsSponsored;
    uint256 cmxUsed;
    uint256 ethEquivalent;
}
```

### PaymasterStats

```solidity
struct PaymasterStats {
    uint256 totalOperationsSponsored;
    uint256 totalCMXUsed;
    uint256 totalETHEquivalent;
}
```

### Layout

```solidity
struct Layout {
    bool initialized;
    address cmxToken;
    address entryPoint;
    address protocolTreasury;
    uint256 exchangeRate;
    uint256 lastRateUpdate;
    mapping(address => bool) rateUpdaters;
    mapping(address => FactoryConfig) factories;
    address[] supportedFactories;
    uint256 cmxBalance;
    uint256 ethDeposit;
    mapping(address => UserStats) userStats;
    PaymasterStats globalStats;
    mapping(bytes32 => uint256) pendingCMXCharges;
    mapping(bytes32 => address) pendingUsers;
    mapping(bytes32 => address) pendingFactories;
    mapping(address => bool) paymasterManagers;
    mapping(address => bool) treasuryManagers;
    bool paused;
    uint256 maxCMXPerOperation;
    uint256 maxOperationsPerUser;
    mapping(address => uint256) userOperationCounts;
    mapping(address => uint256) userLastOperationReset;
    uint256 globalOperationLimit;
    uint256 globalOperationCount;
    uint256 globalLastReset;
    uint256 treasuryTransferThreshold;
    uint256 lastTreasuryTransfer;
    uint256 totalTreasuryTransfers;
}
```

