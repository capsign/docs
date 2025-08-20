# GovernanceCoreFacetImpl
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Governance/governance/facets/GovernanceCoreFacetImpl.sol)

**Inherits:**
IGovernor

This is a concrete implementation that implements all IGovernor interface functions

*Complete implementation contract for ERC7752 lot-based governance*


## State Variables
### _proposals

```solidity
mapping(uint256 => bool) private _proposals;
```


### _proposalEtas

```solidity
mapping(uint256 => uint256) private _proposalEtas;
```


## Functions
### supportsInterface

*See [IERC165-supportsInterface](/dependencies/v4-periphery/lib/v4-core/test/PoolManager.t.sol/contract.PoolManagerTest.md#supportsinterface).*


```solidity
function supportsInterface(bytes4 interfaceId) public view virtual returns (bool);
```

### initializeGovernance

*Initialize governance with basic parameters*


```solidity
function initializeGovernance(
    string memory _name,
    uint256 _votingDelay,
    uint256 _votingPeriod,
    uint256 _proposalThreshold,
    uint256 _quorumNumerator
) external;
```

### configureShareClass

*Configure a share class for governance participation*


```solidity
function configureShareClass(
    address shareClass,
    bool enabled,
    uint256 votingWeight,
    bool requiresLotVoting,
    uint256 minimumHoldingPeriod,
    bool allowDelegation
) external;
```

### castLotVote

*Cast a vote using a specific lot*


```solidity
function castLotVote(uint256 proposalId, bytes32 lotId, uint8 support, string memory reason)
    external
    returns (uint256 weight);
```

### name


```solidity
function name() public pure returns (string memory);
```

### version


```solidity
function version() public pure returns (string memory);
```

### hashProposal


```solidity
function hashProposal(
    address[] memory targets,
    uint256[] memory values,
    bytes[] memory calldatas,
    bytes32 descriptionHash
) public pure returns (uint256);
```

### state


```solidity
function state(uint256 proposalId) public view returns (ProposalState);
```

### proposalSnapshot


```solidity
function proposalSnapshot(uint256 proposalId) public pure returns (uint256);
```

### proposalDeadline


```solidity
function proposalDeadline(uint256 proposalId) public pure returns (uint256);
```

### proposalProposer


```solidity
function proposalProposer(uint256 proposalId) public pure returns (address);
```

### proposalEta


```solidity
function proposalEta(uint256 proposalId) public view returns (uint256);
```

### proposalNeedsQueuing


```solidity
function proposalNeedsQueuing(uint256 proposalId) public pure returns (bool);
```

### proposalThreshold


```solidity
function proposalThreshold() public pure returns (uint256);
```

### votingDelay


```solidity
function votingDelay() public pure returns (uint256);
```

### votingPeriod


```solidity
function votingPeriod() public pure returns (uint256);
```

### quorum


```solidity
function quorum(uint256 timepoint) public pure returns (uint256);
```

### getVotes


```solidity
function getVotes(address account, uint256 timepoint) public pure returns (uint256);
```

### getVotesWithParams


```solidity
function getVotesWithParams(address account, uint256 timepoint, bytes memory params) public pure returns (uint256);
```

### hasVoted


```solidity
function hasVoted(uint256 proposalId, address account) public pure returns (bool);
```

### propose


```solidity
function propose(address[] memory targets, uint256[] memory values, bytes[] memory calldatas, string memory description)
    public
    returns (uint256 proposalId);
```

### execute


```solidity
function execute(address[] memory targets, uint256[] memory values, bytes[] memory calldatas, bytes32 descriptionHash)
    public
    payable
    returns (uint256 proposalId);
```

### cancel


```solidity
function cancel(address[] memory targets, uint256[] memory values, bytes[] memory calldatas, bytes32 descriptionHash)
    public
    returns (uint256 proposalId);
```

### queue


```solidity
function queue(address[] memory targets, uint256[] memory values, bytes[] memory calldatas, bytes32 descriptionHash)
    public
    returns (uint256 proposalId);
```

### castVote


```solidity
function castVote(uint256 proposalId, uint8 support) public returns (uint256 weight);
```

### castVoteWithReason


```solidity
function castVoteWithReason(uint256 proposalId, uint8 support, string memory reason) public returns (uint256 weight);
```

### castVoteWithReasonAndParams


```solidity
function castVoteWithReasonAndParams(uint256 proposalId, uint8 support, string memory reason, bytes memory params)
    public
    returns (uint256 weight);
```

### castVoteBySig


```solidity
function castVoteBySig(uint256 proposalId, uint8 support, address voter, bytes memory signature)
    external
    returns (uint256 balance);
```

### castVoteWithReasonAndParamsBySig


```solidity
function castVoteWithReasonAndParamsBySig(
    uint256 proposalId,
    uint8 support,
    address voter,
    string calldata reason,
    bytes memory params,
    bytes memory signature
) external returns (uint256 balance);
```

### clock


```solidity
function clock() public view returns (uint48);
```

### CLOCK_MODE


```solidity
function CLOCK_MODE() public pure returns (string memory);
```

### COUNTING_MODE


```solidity
function COUNTING_MODE() public pure returns (string memory);
```

