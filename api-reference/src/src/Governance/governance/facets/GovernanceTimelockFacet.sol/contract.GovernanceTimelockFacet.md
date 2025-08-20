# GovernanceTimelockFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Governance/governance/facets/GovernanceTimelockFacet.sol)

**Inherits:**
[UnifiedGovernanceBase](/src/Governance/governance/storage/GovernanceStorage.sol/abstract.UnifiedGovernanceBase.md)

Handles proposal execution delays within the unified governance framework

*Simplified timelock functionality for unified governance*


## State Variables
### proposalExecutionTimes

```solidity
mapping(uint256 => uint256) public proposalExecutionTimes;
```


## Functions
### queueProposal

Queue a proposal for execution after timelock delay


```solidity
function queueProposal(uint256 proposalId) external;
```

### executeQueuedProposal

Execute a queued proposal after timelock delay has passed


```solidity
function executeQueuedProposal(uint256 proposalId) external returns (uint256);
```

### updateTimelockDelay

Update the timelock delay (governance only)


```solidity
function updateTimelockDelay(uint256 newDelay) external;
```

### getProposalExecutionTime

Get the execution time for a queued proposal


```solidity
function getProposalExecutionTime(uint256 proposalId) external view returns (uint256);
```

### isProposalReady

Check if a proposal is ready for execution


```solidity
function isProposalReady(uint256 proposalId) external view returns (bool);
```

### getTimelockDelay

Get current timelock delay


```solidity
function getTimelockDelay() external view returns (uint256);
```

### _proposalSucceeded


```solidity
function _proposalSucceeded(uint256 proposalId) internal view returns (bool);
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
### ProposalQueued

```solidity
event ProposalQueued(uint256 indexed proposalId, uint256 executeTime);
```

### ProposalExecuted

```solidity
event ProposalExecuted(uint256 indexed proposalId);
```

### TimelockDelayUpdated

```solidity
event TimelockDelayUpdated(uint256 oldDelay, uint256 newDelay);
```

