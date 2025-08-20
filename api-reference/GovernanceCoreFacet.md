# GovernanceCoreFacet
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Governance/governance/facets/GovernanceCoreFacet.sol)

**Inherits:**
[UnifiedGovernanceBase](/src/Governance/governance/storage/GovernanceStorage.sol/abstract.UnifiedGovernanceBase.md)

Provides basic proposal creation, voting, and execution functionality

*Core governance functionality for unified OCF-based governance*


## Functions
### propose

Create a new proposal


```solidity
function propose(address[] memory targets, uint256[] memory values, bytes[] memory calldatas, string memory description)
    external
    returns (uint256);
```

### castVote

Cast a vote on a proposal


```solidity
function castVote(uint256 proposalId, uint8 support) external returns (uint256);
```

### castVoteWithReason

Cast a vote with reason


```solidity
function castVoteWithReason(uint256 proposalId, uint8 support, string memory reason) public returns (uint256);
```

### execute

Execute a proposal


```solidity
function execute(uint256 proposalId) external payable returns (uint256);
```

### cancel

Cancel a proposal


```solidity
function cancel(uint256 proposalId) external returns (uint256);
```

### state

Get the state of a proposal


```solidity
function state(uint256 proposalId) public view returns (ProposalState);
```

### getProposal

Get proposal details


```solidity
function getProposal(uint256 proposalId)
    external
    view
    returns (
        uint256 id,
        address proposer,
        uint256 voteStart,
        uint256 voteEnd,
        uint256 forVotes,
        uint256 againstVotes,
        uint256 abstainVotes,
        bool canceled,
        bool executed
    );
```

### getGovernanceParams

Get governance parameters


```solidity
function getGovernanceParams()
    external
    view
    returns (
        uint256 votingDelay,
        uint256 votingPeriod,
        uint256 proposalThreshold,
        uint256 quorumNumerator,
        uint256 quorumDenominator
    );
```

### _quorumReached


```solidity
function _quorumReached(uint256 proposalId) internal view returns (bool);
```

### _voteSucceeded


```solidity
function _voteSucceeded(uint256 proposalId) internal view returns (bool);
```

### _getVotingPower


```solidity
function _getVotingPower(address account) internal view returns (uint256);
```

### _getTotalSupply


```solidity
function _getTotalSupply() internal view returns (uint256);
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
### ProposalCreated

```solidity
event ProposalCreated(
    uint256 indexed proposalId,
    address indexed proposer,
    address[] targets,
    uint256[] values,
    string[] signatures,
    bytes[] calldatas,
    uint256 startTime,
    uint256 endTime,
    string description
);
```

### VoteCast

```solidity
event VoteCast(address indexed voter, uint256 indexed proposalId, uint8 support, uint256 weight, string reason);
```

### ProposalExecuted

```solidity
event ProposalExecuted(uint256 indexed proposalId);
```

### ProposalCanceled

```solidity
event ProposalCanceled(uint256 indexed proposalId);
```

## Enums
### ProposalState

```solidity
enum ProposalState {
    Pending,
    Active,
    Canceled,
    Defeated,
    Succeeded,
    Queued,
    Expired,
    Executed
}
```

