# IBoardGovernance
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Governance/governance/interfaces/IBoardGovernance.sol)

Interface for corporate board governance with roles and committee management


## Functions
### propose

*Create a new board proposal*


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


### proposeBoard

*Create a board-specific proposal*


```solidity
function proposeBoard(
    BoardProposalType proposalType,
    address[] memory targets,
    uint256[] memory values,
    bytes[] memory calldatas,
    string memory description
) external returns (uint256 proposalId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`proposalType`|`BoardProposalType`|Type of board proposal|
|`targets`|`address[]`|Array of target addresses|
|`values`|`uint256[]`|Array of ether values|
|`calldatas`|`bytes[]`|Array of call data|
|`description`|`string`|Proposal description|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`proposalId`|`uint256`|The ID of the created proposal|


### castVote

*Cast a vote as a board member*


```solidity
function castVote(uint256 proposalId, uint8 support) external returns (uint256 balance);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`proposalId`|`uint256`|The proposal ID|
|`support`|`uint8`|The vote type (0=against, 1=for, 2=abstain)|


### castVoteWithReason

*Cast a vote with reason as a board member*


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


### executeiveApproval

*Executive approval for urgent matters*


```solidity
function executeiveApproval(uint256 proposalId, string calldata approvalType) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`proposalId`|`uint256`|The proposal ID|
|`approvalType`|`string`|Type of executive approval|


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


### addBoardMember

*Add a board member*


```solidity
function addBoardMember(address member, BoardRole role) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`member`|`address`|The member address|
|`role`|`BoardRole`|The board role|


### removeBoardMember

*Remove a board member*


```solidity
function removeBoardMember(address member) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`member`|`address`|The member address|


### updateBoardMemberRole

*Update a board member's role*


```solidity
function updateBoardMemberRole(address member, BoardRole newRole) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`member`|`address`|The member address|
|`newRole`|`BoardRole`|The new role|


### grantExecutiveAuthority

*Grant executive authority*


```solidity
function grantExecutiveAuthority(address executive, string calldata authority) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`executive`|`address`|The executive address|
|`authority`|`string`|The authority description|


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


### getBoardMember

*Get board member details*


```solidity
function getBoardMember(address member) external view returns (BoardMember memory boardMember);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`member`|`address`|The member address|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`boardMember`|`BoardMember`|The board member details|


### getBoardMembers

*Get all board members*


```solidity
function getBoardMembers() external view returns (address[] memory members);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`members`|`address[]`|Array of board member addresses|


### isBoardMember

*Check if an address is a board member*


```solidity
function isBoardMember(address member) external view returns (bool isMember);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`member`|`address`|The address to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`isMember`|`bool`|True if the address is a board member|


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


### boardQuorum

*Get board quorum percentage*


```solidity
function boardQuorum() external view returns (uint256 percentage);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`percentage`|`uint256`|The board quorum percentage (1-100)|


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

### BoardProposalCreated

```solidity
event BoardProposalCreated(uint256 indexed proposalId, address indexed proposer, BoardProposalType proposalType);
```

### BoardVoteCast

```solidity
event BoardVoteCast(address indexed voter, uint256 indexed proposalId, uint8 support, BoardRole role, string reason);
```

### ExecutiveApproval

```solidity
event ExecutiveApproval(uint256 indexed proposalId, address indexed executive, string approvalType);
```

### BoardMemberAdded

```solidity
event BoardMemberAdded(address indexed member, BoardRole role);
```

### BoardMemberRemoved

```solidity
event BoardMemberRemoved(address indexed member, BoardRole role);
```

### ExecutiveAuthorityGranted

```solidity
event ExecutiveAuthorityGranted(address indexed executive, string authority);
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
    BoardProposalType proposalType;
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

### BoardMember

```solidity
struct BoardMember {
    address member;
    BoardRole role;
    bool active;
    uint256 joinedAt;
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

### BoardRole

```solidity
enum BoardRole {
    Director,
    IndependentDirector,
    CEO,
    CFO,
    Chairman,
    AuditCommittee,
    CompensationCommittee
}
```

### BoardProposalType

```solidity
enum BoardProposalType {
    EquityGrant,
    ExecutiveCompensation,
    FinancialApproval,
    PolicyChange,
    BoardResolution,
    EmergencyAction
}
```

