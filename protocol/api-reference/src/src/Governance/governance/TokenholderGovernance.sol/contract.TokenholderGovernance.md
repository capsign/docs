# TokenholderGovernance
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Governance/governance/TokenholderGovernance.sol)

**Inherits:**
[Diamond](/src/Diamonds/Diamond.sol/contract.Diamond.md)

Handles shareholder voting, lot-based voting, and major corporate decisions

*Governance diamond for tokenholder/shareholder decisions using unified OCF-based storage*


## Functions
### constructor


```solidity
constructor(string memory _name, uint256 _votingPeriod, uint256 _quorumThreshold)
    Diamond(_createInitParams(_name, _votingPeriod, _quorumThreshold));
```

### _createInitParams

*Create initialization parameters for the diamond*


```solidity
function _createInitParams(string memory _name, uint256 _votingPeriod, uint256 _quorumThreshold)
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

### _getTokenholderVotingSelectors


```solidity
function _getTokenholderVotingSelectors() private pure returns (bytes4[] memory);
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
### TokenholderGovernanceCreated

```solidity
event TokenholderGovernanceCreated(address indexed governance, string name);
```

