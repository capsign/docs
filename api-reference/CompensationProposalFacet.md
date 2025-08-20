# CompensationProposalFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Governance/governance/facets/CompensationProposalFacet.sol)

**Inherits:**
[UnifiedGovernanceBase](/src/Governance/governance/storage/GovernanceStorage.sol/abstract.UnifiedGovernanceBase.md)

Handles executive compensation proposals within the unified governance framework

*Simplified compensation proposal facet for unified governance*


## Functions
### createCompensationProposal

Create a compensation proposal for an executive


```solidity
function createCompensationProposal(
    address executive,
    uint256 amount,
    CompensationType compensationType,
    string memory description
) external returns (uint256 proposalId);
```

### approveCompensation

Approve compensation for an executive (called by governance execution)


```solidity
function approveCompensation(address executive, uint256 amount, CompensationType compensationType) external;
```

### canProposeCompensation

Check if an address can propose compensation


```solidity
function canProposeCompensation(address account) external view returns (bool);
```

### _compensationTypeToString


```solidity
function _compensationTypeToString(CompensationType cType) internal pure returns (string memory);
```

### configureOCFStockClasses


```solidity
function configureOCFStockClasses(GovernanceStorage.OCFStockClass[] calldata stockClasses) external pure override;
```

### configureOCFProposalTypes


```solidity
function configureOCFProposalTypes(GovernanceStorage.OCFProposalTypeConfig[] calldata proposalTypes)
    external
    pure
    override;
```

### updateStakeholderShares


```solidity
function updateStakeholderShares(address stakeholder, string calldata classId, uint256 shares) external pure override;
```

### calculateVotingPower


```solidity
function calculateVotingPower(address stakeholder, string memory proposalType) public pure override returns (uint256);
```

### canVetoProposal


```solidity
function canVetoProposal(address stakeholder, string memory proposalType) public pure override returns (bool);
```

## Events
### CompensationProposalCreated

```solidity
event CompensationProposalCreated(
    uint256 indexed proposalId, address indexed executive, uint256 amount, string compensationType
);
```

### CompensationApproved

```solidity
event CompensationApproved(address indexed executive, uint256 amount, string compensationType);
```

## Enums
### CompensationType

```solidity
enum CompensationType {
    BASE_SALARY,
    BONUS,
    EQUITY_GRANT,
    RETENTION_BONUS,
    SEVERANCE_PACKAGE
}
```

