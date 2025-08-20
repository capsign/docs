# FundUnitFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/facets/FundUnitFacet.sol)

Facet providing fund-specific functionality that extends IssuedAsset

*Diamond facet for fund unit tracking including commitments, contributions, and distributions*


## Functions
### initializeFundUnit

Initialize the fund unit functionality


```solidity
function initializeFundUnit(address fundAddress) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`fundAddress`|`address`|The address of the fund contract|


### recordCommitment

Record a capital commitment


```solidity
function recordCommitment(address investor, uint256 commitment) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor making the commitment|
|`commitment`|`uint256`|The commitment amount|


### recordCapitalContribution

Record a capital contribution


```solidity
function recordCapitalContribution(address investor, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor making the contribution|
|`amount`|`uint256`|The contribution amount|


### recordDistribution

Record a distribution to an investor


```solidity
function recordDistribution(address investor, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor receiving the distribution|
|`amount`|`uint256`|The distribution amount|


### updatePreferredReturn

Update preferred return accrual


```solidity
function updatePreferredReturn(address investor, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor|
|`amount`|`uint256`|The preferred return amount|


### recordDefault

Record a default on a capital call


```solidity
function recordDefault(address investor, uint256 callNumber, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The defaulting investor|
|`callNumber`|`uint256`|The capital call number|
|`amount`|`uint256`|The defaulted amount|


### getUnitHolderInfo

Get unit holder information for an investor


```solidity
function getUnitHolderInfo(address investor) external view returns (FundUnitStorage.UnitHolderInfo memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`FundUnitStorage.UnitHolderInfo`|The unit holder information struct|


### getCommitment

Get commitment amount for an investor


```solidity
function getCommitment(address investor) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The commitment amount|


### getPaidInPercentage

Get paid-in percentage for an investor


```solidity
function getPaidInPercentage(address investor) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The paid-in percentage in basis points (10000 = 100%)|


### getUnfundedCommitment

Get unfunded commitment for an investor


```solidity
function getUnfundedCommitment(address investor) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The unfunded commitment amount|


### getFundMetrics

Get fund metrics


```solidity
function getFundMetrics() external view returns (FundUnitStorage.FundMetrics memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`FundUnitStorage.FundMetrics`|The fund metrics struct|


### fund

Get the associated fund address


```solidity
function fund() external view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The fund contract address|


### isFundUnitInitialized

Check if FundUnit is initialized


```solidity
function isFundUnitInitialized() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if initialized|


### batchRecordCommitments

Batch record commitments for multiple investors


```solidity
function batchRecordCommitments(address[] calldata investors, uint256[] calldata commitments) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investors`|`address[]`|Array of investor addresses|
|`commitments`|`uint256[]`|Array of commitment amounts|


### batchRecordContributions

Batch record contributions for multiple investors


```solidity
function batchRecordContributions(address[] calldata investors, uint256[] calldata amounts) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investors`|`address[]`|Array of investor addresses|
|`amounts`|`uint256[]`|Array of contribution amounts|


### batchGetInvestorInfo

Get summary information for multiple investors


```solidity
function batchGetInvestorInfo(address[] calldata investors)
    external
    view
    returns (uint256[] memory commitments, uint256[] memory paidIn, uint256[] memory distributions);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investors`|`address[]`|Array of investor addresses|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`commitments`|`uint256[]`|Array of commitment amounts|
|`paidIn`|`uint256[]`|Array of paid-in amounts|
|`distributions`|`uint256[]`|Array of distribution amounts|


### getFundPerformanceSummary

Get fund performance summary


```solidity
function getFundPerformanceSummary()
    external
    view
    returns (
        uint256 totalCommitments,
        uint256 totalPaidIn,
        uint256 totalDistributed,
        uint256 totalDefaulted,
        uint256 commitmentUtilization,
        uint256 distributionRatio
    );
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalCommitments`|`uint256`|Total commitments across all investors|
|`totalPaidIn`|`uint256`|Total capital contributed|
|`totalDistributed`|`uint256`|Total distributions made|
|`totalDefaulted`|`uint256`|Total defaults on capital calls|
|`commitmentUtilization`|`uint256`|Percentage of commitments called (basis points)|
|`distributionRatio`|`uint256`|Total distributions / total paid-in (basis points)|


## Events
### CommitmentRecorded

```solidity
event CommitmentRecorded(address indexed investor, uint256 commitment);
```

### CapitalContributed

```solidity
event CapitalContributed(address indexed investor, uint256 amount);
```

### DistributionRecorded

```solidity
event DistributionRecorded(address indexed investor, uint256 amount);
```

### PreferredReturnUpdated

```solidity
event PreferredReturnUpdated(address indexed investor, uint256 amount);
```

### DefaultRecorded

```solidity
event DefaultRecorded(address indexed investor, uint256 callNumber, uint256 amount);
```

