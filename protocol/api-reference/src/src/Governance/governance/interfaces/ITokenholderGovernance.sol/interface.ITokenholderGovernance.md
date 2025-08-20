# ITokenholderGovernance
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Governance/governance/interfaces/ITokenholderGovernance.sol)

Interface for advanced tokenholder governance with ERC7752 lot-based voting


## Functions
### propose

*Create a new proposal*


```solidity
function propose(address[] memory targets, uint256[] memory values, bytes[] memory calldatas, string memory description)
    external
    returns (uint256 proposalId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`targets`|`address[]`|Array of target addresses|
|`values`|`uint256[]`|Array of ether values|
|`calldatas`|`bytes[]`|Array of call data|
|`description`|`string`|Proposal description|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`proposalId`|`uint256`|The ID of the created proposal|


### castVote

*Cast a vote using address-based voting power*


```solidity
function castVote(uint256 proposalId, uint8 support) external returns (uint256 balance);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`proposalId`|`uint256`|The proposal ID|
|`support`|`uint8`|The vote type (0=against, 1=for, 2=abstain)|


### castVoteWithReason

*Cast a vote with reason*


```solidity
function castVoteWithReason(uint256 proposalId, uint8 support, string calldata reason)
    external
    returns (uint256 balance);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`proposalId`|`uint256`|The proposal ID|
|`support`|`uint8`|The vote type|
|`reason`|`string`|Voting reason|


### castLotVote

*Cast vote using specific lot IDs (ERC7752 feature)*


```solidity
function castLotVote(uint256 proposalId, uint256[] calldata lotIds, uint8 support)
    external
    returns (uint256 totalWeight);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`proposalId`|`uint256`|The proposal ID|
|`lotIds`|`uint256[]`|Array of lot IDs to vote with|
|`support`|`uint8`|The vote type|


### queue

*Queue a succeeded proposal for execution*


```solidity
function queue(uint256 proposalId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`proposalId`|`uint256`|The proposal ID|


### execute

*Execute a queued proposal*


```solidity
function execute(uint256 proposalId) external payable;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`proposalId`|`uint256`|The proposal ID|


### cancel

*Cancel a proposal*


```solidity
function cancel(uint256 proposalId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`proposalId`|`uint256`|The proposal ID|


### getProposal

*Get proposal details*


```solidity
function getProposal(uint256 proposalId) external view returns (ProposalCore memory proposal);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`proposalId`|`uint256`|The proposal ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`proposal`|`ProposalCore`|The proposal core details|


### getProposalVotes

*Get proposal votes*


```solidity
function getProposalVotes(uint256 proposalId) external view returns (ProposalVotes memory votes);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`proposalId`|`uint256`|The proposal ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`votes`|`ProposalVotes`|The vote tallies|


### state

*Get proposal state*


```solidity
function state(uint256 proposalId) external view returns (ProposalState state);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`proposalId`|`uint256`|The proposal ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`state`|`ProposalState`|The current proposal state|


### getVotes

*Get voting power for an address at a block*


```solidity
function getVotes(address account, uint256 blockNumber) external view returns (uint256 power);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The account address|
|`blockNumber`|`uint256`|The block number|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`power`|`uint256`|The voting power|


### getLotVotingPower

*Get lot-based voting power*


```solidity
function getLotVotingPower(uint256[] calldata lotIds, uint256 blockNumber)
    external
    view
    returns (uint256 totalWeight);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`lotIds`|`uint256[]`|Array of lot IDs|
|`blockNumber`|`uint256`|The block number|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`totalWeight`|`uint256`|Total voting weight of the lots|


### hasVoted

*Check if an account has voted on a proposal*


```solidity
function hasVoted(uint256 proposalId, address account) external view returns (bool hasVoted);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`proposalId`|`uint256`|The proposal ID|
|`account`|`address`|The account address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`hasVoted`|`bool`|True if the account has voted|


### quorum

*Get quorum requirement for a proposal*


```solidity
function quorum(uint256 proposalId) external view returns (uint256 quorum);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`proposalId`|`uint256`|The proposal ID|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`quorum`|`uint256`|The required quorum|


### votingDelay

*Get voting delay (blocks between proposal and voting start)*


```solidity
function votingDelay() external view returns (uint256 delay);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`delay`|`uint256`|The voting delay in blocks|


### votingPeriod

*Get voting period (blocks for voting duration)*


```solidity
function votingPeriod() external view returns (uint256 period);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`period`|`uint256`|The voting period in blocks|


### proposalThreshold

*Get proposal threshold (minimum tokens to create proposal)*


```solidity
function proposalThreshold() external view returns (uint256 threshold);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`threshold`|`uint256`|The proposal threshold|


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
    uint256 startBlock,
    uint256 endBlock,
    string description
);
```

### VoteCast

```solidity
event VoteCast(address indexed voter, uint256 indexed proposalId, uint8 support, uint256 weight, string reason);
```

### LotVoteCast

```solidity
event LotVoteCast(
    address indexed voter, uint256 indexed proposalId, uint256 indexed lotId, uint8 support, uint256 weight
);
```

### ProposalQueued

```solidity
event ProposalQueued(uint256 indexed proposalId, uint256 eta);
```

### ProposalExecuted

```solidity
event ProposalExecuted(uint256 indexed proposalId);
```

### ProposalCancelled

```solidity
event ProposalCancelled(uint256 indexed proposalId);
```

## Structs
### ProposalCore

```solidity
struct ProposalCore {
    uint256 id;
    address proposer;
    uint256 startBlock;
    uint256 endBlock;
    string description;
    ProposalState state;
}
```

### ProposalVotes

```solidity
struct ProposalVotes {
    uint256 againstVotes;
    uint256 forVotes;
    uint256 abstainVotes;
}
```

### LotVote

```solidity
struct LotVote {
    uint256 lotId;
    VoteType support;
    address voter;
    uint256 weight;
}
```

## Enums
### ProposalState

```solidity
enum ProposalState {
    Pending,
    Active,
    Cancelled,
    Defeated,
    Succeeded,
    Queued,
    Expired,
    Executed
}
```

### VoteType

```solidity
enum VoteType {
    Against,
    For,
    Abstain
}
```

