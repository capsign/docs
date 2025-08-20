# PaymasterManagementFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Paymaster/facets/PaymasterManagementFacet.sol)

Management facet for CMX paymaster diamond

*Handles configuration, factory management, and administrative functions*


## Functions
### onlyPaymasterManager


```solidity
modifier onlyPaymasterManager();
```

### onlyTreasuryManager


```solidity
modifier onlyTreasuryManager();
```

### onlyRateUpdater


```solidity
modifier onlyRateUpdater();
```

### depositCMX

Deposit CMX tokens to the paymaster for fee payments


```solidity
function depositCMX(uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|Amount of CMX tokens to deposit|


### withdrawCMX

Withdraw CMX tokens from the paymaster


```solidity
function withdrawCMX(uint256 amount, address recipient) external onlyPaymasterManager;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|Amount of CMX tokens to withdraw|
|`recipient`|`address`|Address to receive the tokens|


### getCMXBalance

Get CMX token balance of the paymaster


```solidity
function getCMXBalance() external view returns (uint256 balance);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`balance`|`uint256`|Current CMX token balance|


### getUserCMXAllowance

Get user's CMX allowance for the paymaster


```solidity
function getUserCMXAllowance(address user) external view returns (uint256 allowance);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`allowance`|`uint256`|Current CMX allowance|


### updateExchangeRate

Update CMX/ETH exchange rate


```solidity
function updateExchangeRate(uint256 newRate) external onlyRateUpdater;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newRate`|`uint256`|New exchange rate (CMX tokens per 1 ETH, scaled by 1e18)|


### getExchangeRate

Get current CMX/ETH exchange rate


```solidity
function getExchangeRate() external view returns (uint256 rate);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`rate`|`uint256`|Current exchange rate (CMX tokens per 1 ETH, scaled by 1e18)|


### calculateCMXAmount

Calculate CMX amount needed for a given ETH amount


```solidity
function calculateCMXAmount(uint256 ethAmount) external view returns (uint256 cmxAmount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ethAmount`|`uint256`|ETH amount in wei|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`cmxAmount`|`uint256`|Required CMX amount|


### calculateETHEquivalent

Calculate ETH equivalent for a given CMX amount


```solidity
function calculateETHEquivalent(uint256 cmxAmount) external view returns (uint256 ethAmount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`cmxAmount`|`uint256`|CMX amount|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`ethAmount`|`uint256`|Equivalent ETH amount in wei|


### addSupportedFactory

Add a factory to the supported list


```solidity
function addSupportedFactory(address factory, uint256 feeMultiplier) external onlyPaymasterManager;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`factory`|`address`|Factory contract address|
|`feeMultiplier`|`uint256`|Fee multiplier for this factory (scaled by 1e18, 1e18 = 1x)|


### removeSupportedFactory

Remove a factory from the supported list


```solidity
function removeSupportedFactory(address factory) external onlyPaymasterManager;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`factory`|`address`|Factory contract address|


### updateFactoryFeeMultiplier

Update fee multiplier for a factory


```solidity
function updateFactoryFeeMultiplier(address factory, uint256 feeMultiplier) external onlyPaymasterManager;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`factory`|`address`|Factory contract address|
|`feeMultiplier`|`uint256`|New fee multiplier (scaled by 1e18)|


### isFactorySupported

Check if a factory is supported


```solidity
function isFactorySupported(address factory) external view returns (bool supported);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`factory`|`address`|Factory contract address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`supported`|`bool`|Whether the factory is supported|


### getFactoryFeeMultiplier

Get fee multiplier for a factory


```solidity
function getFactoryFeeMultiplier(address factory) external view returns (uint256 multiplier);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`factory`|`address`|Factory contract address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`multiplier`|`uint256`|Fee multiplier (scaled by 1e18)|


### getSupportedFactories

Get all supported factories


```solidity
function getSupportedFactories() external view returns (address[] memory factories);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`factories`|`address[]`|Array of supported factory addresses|


### setProtocolTreasury

Set protocol treasury address


```solidity
function setProtocolTreasury(address treasury) external onlyTreasuryManager;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`treasury`|`address`|New treasury address|


### getProtocolTreasury

Get protocol treasury address


```solidity
function getProtocolTreasury() external view returns (address treasury);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`treasury`|`address`|Current treasury address|


### transferFeesToTreasury

Transfer collected fees to protocol treasury


```solidity
function transferFeesToTreasury(uint256 amount) external onlyTreasuryManager;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|Amount of CMX tokens to transfer|


### getPaymasterStats

Get paymaster statistics


```solidity
function getPaymasterStats()
    external
    view
    returns (uint256 totalOperationsSponsored, uint256 totalCMXUsed, uint256 totalETHEquivalent);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalOperationsSponsored`|`uint256`|Total operations sponsored|
|`totalCMXUsed`|`uint256`|Total CMX tokens used for fees|
|`totalETHEquivalent`|`uint256`|Total ETH equivalent value|


### getUserStats

Get user-specific statistics


```solidity
function getUserStats(address user)
    external
    view
    returns (uint256 operationsSponsored, uint256 cmxUsed, uint256 ethEquivalent);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|User address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`operationsSponsored`|`uint256`|Operations sponsored for this user|
|`cmxUsed`|`uint256`|CMX tokens used by this user|
|`ethEquivalent`|`uint256`|ETH equivalent value for this user|


### addPaymasterManager

Add paymaster manager


```solidity
function addPaymasterManager(address manager) external onlyPaymasterManager;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`manager`|`address`|Address to add as manager|


### removePaymasterManager

Remove paymaster manager


```solidity
function removePaymasterManager(address manager) external onlyPaymasterManager;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`manager`|`address`|Address to remove as manager|


### addTreasuryManager

Add treasury manager


```solidity
function addTreasuryManager(address manager) external onlyTreasuryManager;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`manager`|`address`|Address to add as treasury manager|


### removeTreasuryManager

Remove treasury manager


```solidity
function removeTreasuryManager(address manager) external onlyTreasuryManager;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`manager`|`address`|Address to remove as treasury manager|


### addRateUpdater

Add rate updater


```solidity
function addRateUpdater(address updater) external onlyPaymasterManager;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`updater`|`address`|Address to add as rate updater|


### removeRateUpdater

Remove rate updater


```solidity
function removeRateUpdater(address updater) external onlyPaymasterManager;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`updater`|`address`|Address to remove as rate updater|


### updateMaxCMXPerOperation

Update maximum CMX per operation


```solidity
function updateMaxCMXPerOperation(uint256 maxCMX) external onlyPaymasterManager;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`maxCMX`|`uint256`|New maximum CMX per operation|


### updateMaxOperationsPerUser

Update maximum operations per user per day


```solidity
function updateMaxOperationsPerUser(uint256 maxOps) external onlyPaymasterManager;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`maxOps`|`uint256`|New maximum operations per user|


### updateTreasuryTransferThreshold

Update treasury transfer threshold


```solidity
function updateTreasuryTransferThreshold(uint256 threshold) external onlyTreasuryManager;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`threshold`|`uint256`|New threshold for auto-transfer to treasury|


### getConfiguration

Get current configuration


```solidity
function getConfiguration()
    external
    view
    returns (uint256 maxCMXPerOperation, uint256 maxOperationsPerUser, uint256 treasuryTransferThreshold);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`maxCMXPerOperation`|`uint256`|Maximum CMX per operation|
|`maxOperationsPerUser`|`uint256`|Maximum operations per user per day|
|`treasuryTransferThreshold`|`uint256`|Treasury transfer threshold|


