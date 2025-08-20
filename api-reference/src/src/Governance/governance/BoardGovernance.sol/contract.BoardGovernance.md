# BoardGovernance
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Governance/governance/BoardGovernance.sol)

**Inherits:**
[Diamond](/src/Diamonds/Diamond.sol/contract.Diamond.md)

Handles board voting, executive approvals, and corporate governance

*Governance diamond for board-level decisions using unified OCF-based storage*


## Functions
### constructor


```solidity
constructor(string memory _name, uint256 _boardVotingPeriod, uint256 _boardQuorum)
    Diamond(_createInitParams(_name, _boardVotingPeriod, _boardQuorum));
```

### _createInitParams

*Create initialization parameters for the diamond*


```solidity
function _createInitParams(string memory _name, uint256 _boardVotingPeriod, uint256 _boardQuorum)
    private
    returns (Diamond.InitParams memory);
```

### _getDiamondCutSelectors


```solidity
function _getDiamondCutSelectors() private pure returns (bytes4[] memory);
```

### _getDiamondLoupeSelectors


```solidity
function _getDiamondLoupeSelectors() private pure returns (bytes4[] memory);
```

### _getAccessControlSelectors


```solidity
function _getAccessControlSelectors() private pure returns (bytes4[] memory);
```

### _getBoardVotingSelectors


```solidity
function _getBoardVotingSelectors() private pure returns (bytes4[] memory);
```

### _getGovernanceTimelockSelectors


```solidity
function _getGovernanceTimelockSelectors() private pure returns (bytes4[] memory);
```

### receive


```solidity
receive() external payable;
```

## Events
### BoardGovernanceCreated

```solidity
event BoardGovernanceCreated(address indexed governance, string name);
```

