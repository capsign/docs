# DistributionFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Funds/fund/facets/DistributionFacet.sol)

**Inherits:**
AccessControl

**Author:**
CMX Protocol Team

Facet responsible for managing fund distributions to investors

*This facet handles all distribution-related operations including:
- Return of capital distributions
- Preferred return distributions
- Profit distributions (carried interest)
- Distribution calculations and waterfall logic
- Tax reporting and compliance
Key Features:
- Waterfall distribution logic
- Pro-rata distribution calculations
- Multiple distribution types support
- Tax-efficient distribution strategies
- Comprehensive distribution reporting
- Automated distribution processing
- Now includes event bubbling to diamond level for subgraph indexing
Distribution Types:
- Return of Capital: Returns invested capital to LPs
- Preferred Return: Minimum return threshold for LPs
- Profit Distribution: Carried interest split between GP/LP
- Interim Distributions: Periodic distributions during fund life
- Final Distributions: Liquidation distributions*


## Functions
### declareDistribution

Declare a new distribution to investors

*Creates a distribution record and calculates individual investor amounts*


```solidity
function declareDistribution(
    DistributionType distributionType,
    uint256 totalAmount,
    uint256 recordDate,
    uint256 paymentDate
) external returns (uint256 distributionId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`distributionType`|`DistributionType`|Type of distribution being declared|
|`totalAmount`|`uint256`|Total amount to be distributed|
|`recordDate`|`uint256`|Date for determining eligible investors|
|`paymentDate`|`uint256`|Date when distribution will be paid|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`distributionId`|`uint256`|Unique identifier for the distribution|


### processDistribution

Process distribution payments to all eligible investors

*Executes the distribution to all investors who haven't been paid yet*


```solidity
function processDistribution(uint256 distributionId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`distributionId`|`uint256`|ID of the distribution to process|


### payDistributionToInvestor

Pay distribution to a specific investor

*Pays the calculated distribution amount to a single investor*


```solidity
function payDistributionToInvestor(uint256 distributionId, address investor) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`distributionId`|`uint256`|ID of the distribution|
|`investor`|`address`|Address of the investor to pay|


### calculateInvestorDistribution

Calculate distribution amount for a specific investor

*Returns the amount an investor would receive for a distribution*


```solidity
function calculateInvestorDistribution(uint256 distributionId, address investor)
    external
    view
    returns (uint256 amount);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`distributionId`|`uint256`|ID of the distribution|
|`investor`|`address`|Address of the investor|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|Amount the investor would receive|


### getDistribution

Get distribution details


```solidity
function getDistribution(uint256 distributionId)
    external
    view
    returns (
        uint256 id,
        DistributionType distributionType,
        uint256 totalAmount,
        uint256 recordDate,
        uint256 paymentDate,
        bool isComplete,
        uint256 returnOfCapital,
        uint256 preferredReturn,
        uint256 profitDistribution
    );
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`distributionId`|`uint256`|ID of the distribution to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`id`|`uint256`|Distribution ID|
|`distributionType`|`DistributionType`|Type of distribution|
|`totalAmount`|`uint256`|Total amount distributed|
|`recordDate`|`uint256`|Record date for eligibility|
|`paymentDate`|`uint256`|Payment date|
|`isComplete`|`bool`|Whether distribution is complete|
|`returnOfCapital`|`uint256`|Return of capital amount|
|`preferredReturn`|`uint256`|Preferred return amount|
|`profitDistribution`|`uint256`|Profit distribution amount|


### getTotalDistributions

Get total distributions made to date


```solidity
function getTotalDistributions() external view returns (uint256 totalDistributions);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalDistributions`|`uint256`|Number of distributions declared|


### hasInvestorBeenPaid

Check if an investor has been paid for a distribution


```solidity
function hasInvestorBeenPaid(uint256 distributionId, address investor) external view returns (bool isPaid);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`distributionId`|`uint256`|ID of the distribution|
|`investor`|`address`|Address of the investor|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`isPaid`|`bool`|Whether the investor has been paid|


### getTotalInvestorDistributions

Get total amount distributed to an investor across all distributions


```solidity
function getTotalInvestorDistributions(address investor) external view returns (uint256 totalReceived);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|Address of the investor|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalReceived`|`uint256`|Total amount received by the investor|


### getInvestorDistributionHistory

Get distribution history for an investor


```solidity
function getInvestorDistributionHistory(address investor)
    external
    view
    returns (uint256[] memory distributionIds, uint256[] memory amounts);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|Address of the investor|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`distributionIds`|`uint256[]`|Array of distribution IDs the investor participated in|
|`amounts`|`uint256[]`|Array of amounts received for each distribution|


### _calculateDistributionWaterfall

Calculate distribution waterfall based on type and amount

*Implements the fund's distribution waterfall logic*


```solidity
function _calculateDistributionWaterfall(DistributionType distributionType, uint256 totalAmount)
    internal
    view
    returns (uint256 returnOfCapital, uint256 preferredReturn, uint256 profitDistribution);
```

### _calculateInvestorDistributions

Calculate individual investor distributions

*Calculates pro-rata distributions for each investor*


```solidity
function _calculateInvestorDistributions(uint256 distributionId) internal;
```

### _payDistributionToInvestor

Pay distribution to a specific investor

*Executes the actual payment to an investor*


```solidity
function _payDistributionToInvestor(uint256 distributionId, address investor) internal;
```

### _calculatePreferredReturnOwed

Calculate preferred return owed to investors

*Calculates cumulative preferred return based on time and rate*


```solidity
function _calculatePreferredReturnOwed() internal view returns (uint256 preferredReturnOwed);
```

### _emitWaterfallDetails

Emit waterfall processing details

*Emits detailed waterfall information for transparency*


```solidity
function _emitWaterfallDetails(uint256 distributionId, address investor, uint256 totalAmount) internal;
```

## Events
### DistributionDeclared
Emitted when a distribution is declared


```solidity
event DistributionDeclared(
    uint256 indexed distributionId,
    DistributionType distributionType,
    uint256 totalAmount,
    uint256 recordDate,
    uint256 paymentDate,
    uint256 timestamp
);
```

### DistributionPaid
Emitted when a distribution is paid to an investor


```solidity
event DistributionPaid(
    uint256 indexed distributionId,
    address indexed investor,
    uint256 amount,
    DistributionType distributionType,
    uint256 timestamp
);
```

### DistributionCalculated
Emitted when distribution calculations are updated


```solidity
event DistributionCalculated(
    uint256 indexed distributionId,
    uint256 totalDistributable,
    uint256 returnOfCapital,
    uint256 preferredReturn,
    uint256 profitDistribution,
    uint256 timestamp
);
```

### CarriedInterestCalculated
Emitted when carried interest is calculated


```solidity
event CarriedInterestCalculated(
    uint256 indexed distributionId,
    uint256 totalProfit,
    uint256 gpCarriedInterest,
    uint256 lpProfitShare,
    uint256 timestamp
);
```

### WaterfallProcessed
Emitted when distribution waterfall is processed


```solidity
event WaterfallProcessed(
    uint256 indexed distributionId,
    address indexed investor,
    uint256 returnOfCapital,
    uint256 preferredReturn,
    uint256 profitShare,
    uint256 totalReceived,
    uint256 timestamp
);
```

## Errors
### DistributionFacet__InvalidAmount

```solidity
error DistributionFacet__InvalidAmount();
```

### DistributionFacet__InvalidDistributionType

```solidity
error DistributionFacet__InvalidDistributionType();
```

### DistributionFacet__InsufficientFunds

```solidity
error DistributionFacet__InsufficientFunds();
```

### DistributionFacet__DistributionNotFound

```solidity
error DistributionFacet__DistributionNotFound();
```

### DistributionFacet__DistributionAlreadyPaid

```solidity
error DistributionFacet__DistributionAlreadyPaid();
```

### DistributionFacet__DistributionNotReady

```solidity
error DistributionFacet__DistributionNotReady();
```

### DistributionFacet__InvalidInvestor

```solidity
error DistributionFacet__InvalidInvestor();
```

### DistributionFacet__FundNotActive

```solidity
error DistributionFacet__FundNotActive();
```

### DistributionFacet__InvalidRecordDate

```solidity
error DistributionFacet__InvalidRecordDate();
```

### DistributionFacet__InvalidPaymentDate

```solidity
error DistributionFacet__InvalidPaymentDate();
```

## Enums
### DistributionType

```solidity
enum DistributionType {
    RETURN_OF_CAPITAL,
    PREFERRED_RETURN,
    PROFIT_DISTRIBUTION,
    INTERIM_DISTRIBUTION,
    FINAL_DISTRIBUTION
}
```

