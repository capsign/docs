# FundUnitStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/storage/FundUnitStorage.sol)

Diamond storage library for FundUnit functionality that extends IssuedAsset

*Uses deterministic storage slots to avoid storage collisions with IssuedAsset*


## State Variables
### FUND_UNIT_STORAGE_POSITION

```solidity
bytes32 constant FUND_UNIT_STORAGE_POSITION = keccak256("capsign.storage.fund_unit");
```


## Functions
### layout

Get the FundUnit storage layout


```solidity
function layout() internal pure returns (Layout storage l);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`l`|`Layout`|The storage layout struct|


### initializeFundUnitStorage

Initialize the FundUnit storage with fund-specific data


```solidity
function initializeFundUnitStorage(address fundAddress) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`fundAddress`|`address`|The address of the fund contract|


### requireFund

Require that the caller is the associated fund


```solidity
function requireFund() internal view;
```

### requireFundUnitInitialized

Require that FundUnit is initialized


```solidity
function requireFundUnitInitialized() internal view;
```

### recordCommitmentInternal

Record a capital commitment for an investor


```solidity
function recordCommitmentInternal(address investor, uint256 commitment) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`commitment`|`uint256`|The commitment amount to add|


### recordCapitalContributionInternal

Record a capital contribution for an investor


```solidity
function recordCapitalContributionInternal(address investor, uint256 amount) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`amount`|`uint256`|The contribution amount to add|


### recordDistributionInternal

Record a distribution to an investor


```solidity
function recordDistributionInternal(address investor, uint256 amount) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`amount`|`uint256`|The distribution amount to add|


### updatePreferredReturnInternal

Update preferred return accrual for an investor


```solidity
function updatePreferredReturnInternal(address investor, uint256 amount) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`amount`|`uint256`|The new preferred return amount (not additive)|


### recordDefaultInternal

Record a default on a capital call


```solidity
function recordDefaultInternal(address investor, uint256 amount) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`amount`|`uint256`|The defaulted amount to add|


### getUnitHolderInfoInternal

Get unit holder information for an investor


```solidity
function getUnitHolderInfoInternal(address investor) internal view returns (UnitHolderInfo memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`UnitHolderInfo`|The unit holder information struct|


### getCommitmentInternal

Get commitment amount for an investor


```solidity
function getCommitmentInternal(address investor) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The commitment amount|


### getPaidInPercentageInternal

Calculate paid-in percentage for an investor


```solidity
function getPaidInPercentageInternal(address investor) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The paid-in percentage in basis points (10000 = 100%)|


### getUnfundedCommitmentInternal

Get unfunded commitment for an investor


```solidity
function getUnfundedCommitmentInternal(address investor) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The unfunded commitment amount|


### getFundMetricsInternal

Get fund metrics


```solidity
function getFundMetricsInternal() internal view returns (FundMetrics memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`FundMetrics`|The fund metrics struct|


### getFundInternal

Get the associated fund address


```solidity
function getFundInternal() internal view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The fund contract address|


### isFundUnitInitialized

Check if FundUnit is initialized


```solidity
function isFundUnitInitialized() internal view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if initialized|


### batchRecordCommitmentsInternal

Batch update multiple investor commitments


```solidity
function batchRecordCommitmentsInternal(address[] memory investors, uint256[] memory commitments) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investors`|`address[]`|Array of investor addresses|
|`commitments`|`uint256[]`|Array of commitment amounts|


### batchRecordContributionsInternal

Batch update multiple investor contributions


```solidity
function batchRecordContributionsInternal(address[] memory investors, uint256[] memory amounts) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investors`|`address[]`|Array of investor addresses|
|`amounts`|`uint256[]`|Array of contribution amounts|


## Structs
### UnitHolderInfo

```solidity
struct UnitHolderInfo {
    uint256 commitmentAmount;
    uint256 paidInAmount;
    uint256 distributedAmount;
    uint256 preferredReturnAccrued;
    uint256 capitalCallsDefaulted;
}
```

### FundMetrics

```solidity
struct FundMetrics {
    uint256 totalCommitments;
    uint256 totalPaidIn;
    uint256 totalDistributed;
    uint256 totalDefaulted;
}
```

### Layout

```solidity
struct Layout {
    address fund;
    mapping(address => UnitHolderInfo) unitHolderInfo;
    FundMetrics fundMetrics;
    bool fundUnitInitialized;
}
```

