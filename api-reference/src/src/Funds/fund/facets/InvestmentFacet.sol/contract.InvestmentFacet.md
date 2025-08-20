# InvestmentFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Funds/fund/facets/InvestmentFacet.sol)

**Inherits:**
AccessControl

**Author:**
CMX Protocol Team

Facet responsible for managing fund investments and portfolio operations

*This facet handles all investment-related operations including:
- Portfolio investment management
- Investment tracking and valuation
- NAV calculations and reporting
- Investment approval workflows
- Performance tracking and analytics
Key Features:
- Multi-asset portfolio management
- Investment approval and execution
- Real-time NAV calculations
- Investment performance tracking
- Risk management and limits
- Comprehensive investment reporting
- Now includes event bubbling to diamond level for subgraph indexing
Investment Types Supported:
- Direct token investments
- Liquidity pool positions
- Yield farming strategies
- DeFi protocol investments
- Cross-chain investments*


## Functions
### makeInvestment

Make a new investment from fund assets

*Executes an investment strategy using fund capital*


```solidity
function makeInvestment(
    address asset,
    uint256 amount,
    address target,
    IUniversalFund.InvestmentType investmentType,
    bytes calldata metadata
) external returns (bytes32 investmentId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset to invest|
|`amount`|`uint256`|Amount to invest|
|`target`|`address`|Investment target (protocol, pool, etc.)|
|`investmentType`|`IUniversalFund.InvestmentType`|Type of investment being made|
|`metadata`|`bytes`|Additional investment metadata|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`investmentId`|`bytes32`|Unique identifier for the investment|


### divestInvestment

Divest from an existing investment

*Liquidates an investment position and returns proceeds to fund*


```solidity
function divestInvestment(bytes32 investmentId, uint256 amount) external returns (uint256 proceeds);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investmentId`|`bytes32`|ID of the investment to divest|
|`amount`|`uint256`|Amount to divest (0 for full divestment)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`proceeds`|`uint256`|Amount received from divestment|


### rebalancePortfolio

Rebalance the portfolio according to target allocations

*Adjusts portfolio positions to match target allocation percentages*


```solidity
function rebalancePortfolio(uint256[] calldata targetAllocations, address[] calldata assets) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`targetAllocations`|`uint256[]`|Array of target allocation percentages (basis points)|
|`assets`|`address[]`|Array of assets corresponding to allocations|


### getInvestment

Get details of a specific investment


```solidity
function getInvestment(bytes32 investmentId) external view returns (IUniversalFund.Investment memory investment);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investmentId`|`bytes32`|ID of the investment to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`investment`|`IUniversalFund.Investment`|Complete investment details|


### getActiveInvestments

Get all active investments


```solidity
function getActiveInvestments() external view returns (bytes32[] memory investments);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`investments`|`bytes32[]`|Array of all active investment IDs|


### getPortfolioAllocation

Get portfolio allocation for a specific asset


```solidity
function getPortfolioAllocation(address asset) external view returns (uint256 allocation);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Address of the asset to query|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`allocation`|`uint256`|Current allocation amount for the asset|


### getNAVPerShare

Get current NAV per share


```solidity
function getNAVPerShare() external view returns (uint256 nav);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`nav`|`uint256`|Current net asset value per share|


### getTotalPortfolioValue

Get total portfolio value


```solidity
function getTotalPortfolioValue() external view returns (uint256 totalValue);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalValue`|`uint256`|Current total portfolio value|


### getInvestmentPerformance

Get investment performance metrics


```solidity
function getInvestmentPerformance()
    external
    view
    returns (uint256 totalInvested, uint256 currentValue, int256 unrealizedPnL, int256 realizedPnL);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalInvested`|`uint256`|Total amount invested|
|`currentValue`|`uint256`|Current portfolio value|
|`unrealizedPnL`|`int256`|Unrealized profit/loss|
|`realizedPnL`|`int256`|Realized profit/loss|


### updateNAV

Update the fund's NAV

*Recalculates NAV based on current portfolio values*


```solidity
function updateNAV() external;
```

### setInvestmentLimits

Set investment limits for an asset


```solidity
function setInvestmentLimits(address asset, uint256 maxAllocation, uint256 maxSingleInvestment) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`asset`|`address`|Asset to set limits for|
|`maxAllocation`|`uint256`|Maximum allocation percentage (basis points)|
|`maxSingleInvestment`|`uint256`|Maximum single investment amount|


### _executeInvestment

Execute an investment transaction

*Placeholder for actual investment execution logic*


```solidity
function _executeInvestment(
    address asset,
    uint256 amount,
    address target,
    IUniversalFund.InvestmentType investmentType,
    bytes calldata metadata
) internal;
```

### _executeDivestment

Execute a divestment transaction

*Placeholder for actual divestment execution logic*


```solidity
function _executeDivestment(bytes32 investmentId, uint256 amount) internal returns (uint256 proceeds);
```

### _executeRebalancing

Execute portfolio rebalancing

*Placeholder for actual rebalancing logic*


```solidity
function _executeRebalancing(uint256[] calldata targetAllocations, address[] calldata assets) internal;
```

### _calculateTotalPortfolioValue

Calculate total portfolio value

*Sums up all investment values plus available cash*


```solidity
function _calculateTotalPortfolioValue() internal view returns (uint256 totalValue);
```

### _updateNAV

Update NAV calculations

*Recalculates NAV per share based on current portfolio value*


```solidity
function _updateNAV() internal;
```

### _checkInvestmentLimits

Check investment limits before making an investment

*Validates that investment doesn't exceed configured limits*


```solidity
function _checkInvestmentLimits(address asset, uint256 amount) internal view;
```

### _removeActiveInvestment

Remove an investment from the active investments array

*Helper function to maintain active investments list*


```solidity
function _removeActiveInvestment(bytes32 investmentId) internal;
```

## Events
### InvestmentMade
Emitted when a new investment is made


```solidity
event InvestmentMade(address indexed asset, address indexed target, bytes32 indexed investmentId, uint256 amount);
```

### InvestmentDivested
Emitted when an investment is divested


```solidity
event InvestmentDivested(bytes32 indexed investmentId, address indexed asset, uint256 amount, uint256 proceeds);
```

### PortfolioRebalanced
Emitted when portfolio is rebalanced


```solidity
event PortfolioRebalanced(address indexed initiator, uint256 totalValueBefore, uint256 totalValueAfter);
```

### NAVUpdated
Emitted when NAV is updated


```solidity
event NAVUpdated(uint256 newNAV, uint256 totalAssets, uint256 totalLiabilities);
```

### InvestmentLimitsUpdated
Emitted when investment limits are updated


```solidity
event InvestmentLimitsUpdated(address indexed asset, uint256 maxAllocation, uint256 maxSingleInvestment);
```

## Errors
### InvestmentFacet__InvalidAsset

```solidity
error InvestmentFacet__InvalidAsset();
```

### InvestmentFacet__InvalidAmount

```solidity
error InvestmentFacet__InvalidAmount();
```

### InvestmentFacet__InvalidTarget

```solidity
error InvestmentFacet__InvalidTarget();
```

### InvestmentFacet__InsufficientBalance

```solidity
error InvestmentFacet__InsufficientBalance();
```

### InvestmentFacet__ExceedsAllocationLimit

```solidity
error InvestmentFacet__ExceedsAllocationLimit();
```

### InvestmentFacet__ExceedsInvestmentLimit

```solidity
error InvestmentFacet__ExceedsInvestmentLimit();
```

### InvestmentFacet__InvestmentNotFound

```solidity
error InvestmentFacet__InvestmentNotFound();
```

### InvestmentFacet__UnauthorizedInvestment

```solidity
error InvestmentFacet__UnauthorizedInvestment();
```

### InvestmentFacet__InvalidInvestmentStatus

```solidity
error InvestmentFacet__InvalidInvestmentStatus();
```

### InvestmentFacet__PortfolioRebalanceRequired

```solidity
error InvestmentFacet__PortfolioRebalanceRequired();
```

