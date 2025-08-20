# CapitalAccountFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Funds/fund/facets/CapitalAccountFacet.sol)

**Inherits:**
AccessControl

**Author:**
CMX Protocol Team

Diamond facet for managing capital accounts and investor tracking

*This facet handles all capital account operations including:
- Investor account creation and management
- Capital commitment and contribution tracking
- Distribution recording and audit trails
- Management fee and carried interest allocation
- Comprehensive investor analytics
Key Features:
- Complete audit trail for all transactions
- Investor type differentiation (LP/GP)
- Pro-rata calculations for distributions
- Management fee and carry tracking
- Account lifecycle management
- Comprehensive reporting capabilities
Integration:
- Works with CapitalCallFacet for contribution processing
- Integrates with DistributionFacet for distribution tracking
- Supports InvestmentFacet for performance calculations
- Now includes event bubbling to diamond level for subgraph indexing*


## State Variables
### FUND_ROLE
Role for fund operations


```solidity
bytes32 public constant FUND_ROLE = keccak256("FUND_ROLE");
```


### OPERATOR_ROLE
Role for operators


```solidity
bytes32 public constant OPERATOR_ROLE = keccak256("OPERATOR_ROLE");
```


### GP_ROLE
Role for general partner


```solidity
bytes32 public constant GP_ROLE = keccak256("GP_ROLE");
```


### PLACEMENT_AGENT_ROLE
Role for placement agents


```solidity
bytes32 public constant PLACEMENT_AGENT_ROLE = keccak256("PLACEMENT_AGENT_ROLE");
```


### INVESTOR_TYPE_LP

```solidity
uint8 public constant INVESTOR_TYPE_LP = 0;
```


### INVESTOR_TYPE_GP

```solidity
uint8 public constant INVESTOR_TYPE_GP = 1;
```


### ENTRY_TYPE_COMMITMENT

```solidity
uint8 public constant ENTRY_TYPE_COMMITMENT = 0;
```


### ENTRY_TYPE_CONTRIBUTION

```solidity
uint8 public constant ENTRY_TYPE_CONTRIBUTION = 1;
```


### ENTRY_TYPE_DISTRIBUTION

```solidity
uint8 public constant ENTRY_TYPE_DISTRIBUTION = 2;
```


### ENTRY_TYPE_FEE

```solidity
uint8 public constant ENTRY_TYPE_FEE = 3;
```


### ENTRY_TYPE_CARRY

```solidity
uint8 public constant ENTRY_TYPE_CARRY = 4;
```


## Functions
### onlyFundOrGP


```solidity
modifier onlyFundOrGP();
```

### onlyOperatorOrGP


```solidity
modifier onlyOperatorOrGP();
```

### validInvestor


```solidity
modifier validInvestor(address investor);
```

### createAccount

Create a new capital account for an investor

*Creates account with initial commitment and sets up tracking*


```solidity
function createAccount(address investor, uint8 investorType, uint256 commitment) external onlyFundOrGP;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`investorType`|`uint8`|Whether LP (0) or GP (1)|
|`commitment`|`uint256`|Initial commitment amount|


### recordCommitment

Record additional commitment from an investor

*Adds to existing commitment and updates tracking*


```solidity
function recordCommitment(address investor, uint256 amount) external onlyFundOrGP validInvestor(investor);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`amount`|`uint256`|Additional commitment amount|


### recordContribution

Record a capital contribution from an investor

*Records contribution and reduces unfunded commitment*


```solidity
function recordContribution(address investor, uint256 amount, uint256 callNumber)
    external
    onlyFundOrGP
    validInvestor(investor);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`amount`|`uint256`|Contribution amount|
|`callNumber`|`uint256`|The capital call number|


### recordDistribution

Record a distribution to an investor

*Records distribution and updates totals*


```solidity
function recordDistribution(address investor, uint256 amount, uint256 distributionType)
    external
    onlyFundOrGP
    validInvestor(investor);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`amount`|`uint256`|Distribution amount|
|`distributionType`|`uint256`|Type of distribution (0=capital, 1=preferred, 2=profit)|


### deductManagementFee

Deduct management fee from an investor's account

*Records fee deduction for tracking purposes*


```solidity
function deductManagementFee(address investor, uint256 amount) external onlyFundOrGP validInvestor(investor);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`amount`|`uint256`|Fee amount|


### allocateCarriedInterest

Allocate carried interest to a GP

*Records carried interest allocation for GP*


```solidity
function allocateCarriedInterest(address gp, uint256 amount) external onlyFundOrGP validInvestor(gp);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`gp`|`address`|The GP address|
|`amount`|`uint256`|Carry amount|


### updatePreferredReturn

Update preferred return accrual for an investor

*Updates the preferred return amount owed*


```solidity
function updatePreferredReturn(address investor, uint256 amount) external onlyFundOrGP validInvestor(investor);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`amount`|`uint256`|Preferred return amount|


### closeAccount

Close an investor's account

*Deactivates account after ensuring no unfunded commitments*


```solidity
function closeAccount(address investor) external onlyOperatorOrGP validInvestor(investor);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|


### getAccount

Get complete capital account information for an investor


```solidity
function getAccount(address investor) external view returns (UniversalFundStorage.CapitalAccount memory account);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`account`|`UniversalFundStorage.CapitalAccount`|Complete capital account details|


### getAccountEntries

Get account entries (audit trail) for an investor


```solidity
function getAccountEntries(address investor, uint256 offset, uint256 limit)
    external
    view
    returns (UniversalFundStorage.AccountEntry[] memory entries);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`offset`|`uint256`|Starting index for pagination|
|`limit`|`uint256`|Maximum number of entries to return|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`entries`|`UniversalFundStorage.AccountEntry[]`|Array of account entries|


### getTotalCommitments

Get total commitments across all investors


```solidity
function getTotalCommitments() external view returns (uint256 totalCommitments);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalCommitments`|`uint256`|Total committed capital|


### getTotalContributions

Get total contributions across all investors


```solidity
function getTotalContributions() external view returns (uint256 totalContributions);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalContributions`|`uint256`|Total contributed capital|


### getTotalDistributions

Get total distributions across all investors


```solidity
function getTotalDistributions() external view returns (uint256 totalDistributions);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalDistributions`|`uint256`|Total distributed capital|


### getUnfundedCommitments

Get total unfunded commitments


```solidity
function getUnfundedCommitments() external view returns (uint256 unfundedCommitments);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`unfundedCommitments`|`uint256`|Total unfunded commitments|


### getInvestorsByType

Get investors by type (LP or GP)


```solidity
function getInvestorsByType(uint8 investorType) external view returns (address[] memory investors);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investorType`|`uint8`|Type of investor (0=LP, 1=GP)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`investors`|`address[]`|Array of investor addresses|


### calculateOwnershipPercentage

Calculate ownership percentage for an investor


```solidity
function calculateOwnershipPercentage(address investor) external view returns (uint256 percentage);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`percentage`|`uint256`|Ownership percentage in basis points (10000 = 100%)|


### calculateProRataShare

Calculate pro-rata share for an investor


```solidity
function calculateProRataShare(address investor, uint256 totalAmount) external view returns (uint256 share);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`totalAmount`|`uint256`|Total amount to distribute|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`share`|`uint256`|Pro-rata share amount|


### isInvestor

Check if an address is an active investor


```solidity
function isInvestor(address account) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|isInvestor True if the address is an active investor|


### getActiveInvestorCount

Get the number of active investors


```solidity
function getActiveInvestorCount() external view returns (uint256 count);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`count`|`uint256`|Number of active investors|


### _recordEntry

Record an account entry for audit trail


```solidity
function _recordEntry(address investor, uint8 entryType, uint256 amount, uint256 balance, string memory description)
    internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`investor`|`address`|The investor address|
|`entryType`|`uint8`|Type of entry|
|`amount`|`uint256`|Transaction amount|
|`balance`|`uint256`|New balance after transaction|
|`description`|`string`|Human-readable description|


### _toString

Convert uint256 to string


```solidity
function _toString(uint256 value) internal pure returns (string memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`value`|`uint256`|The number to convert|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|String representation of the number|


## Events
### AccountCreated
Emitted when a new capital account is created


```solidity
event AccountCreated(address indexed investor, uint8 investorType, uint256 commitment);
```

### CapitalCommitted
Emitted when capital is committed


```solidity
event CapitalCommitted(address indexed investor, uint256 amount);
```

### CapitalContributed
Emitted when capital is contributed


```solidity
event CapitalContributed(address indexed investor, uint256 amount, uint256 callNumber);
```

### DistributionRecorded
Emitted when distribution is recorded


```solidity
event DistributionRecorded(address indexed investor, uint256 amount, uint256 distributionType);
```

### ManagementFeeDeducted
Emitted when management fee is deducted


```solidity
event ManagementFeeDeducted(address indexed investor, uint256 amount);
```

### CarriedInterestAllocated
Emitted when carried interest is allocated


```solidity
event CarriedInterestAllocated(address indexed gp, uint256 amount);
```

### AccountClosed
Emitted when an account is closed


```solidity
event AccountClosed(address indexed investor);
```

## Errors
### CapitalAccountFacet__InvalidInvestor

```solidity
error CapitalAccountFacet__InvalidInvestor();
```

### CapitalAccountFacet__AccountExists

```solidity
error CapitalAccountFacet__AccountExists();
```

### CapitalAccountFacet__AccountNotFound

```solidity
error CapitalAccountFacet__AccountNotFound();
```

### CapitalAccountFacet__InactiveAccount

```solidity
error CapitalAccountFacet__InactiveAccount();
```

### CapitalAccountFacet__InvalidCommitment

```solidity
error CapitalAccountFacet__InvalidCommitment();
```

### CapitalAccountFacet__InvalidAmount

```solidity
error CapitalAccountFacet__InvalidAmount();
```

### CapitalAccountFacet__ExceedsUnfunded

```solidity
error CapitalAccountFacet__ExceedsUnfunded();
```

### CapitalAccountFacet__NotGP

```solidity
error CapitalAccountFacet__NotGP();
```

### CapitalAccountFacet__HasUnfundedCommitment

```solidity
error CapitalAccountFacet__HasUnfundedCommitment();
```

### CapitalAccountFacet__Unauthorized

```solidity
error CapitalAccountFacet__Unauthorized();
```

