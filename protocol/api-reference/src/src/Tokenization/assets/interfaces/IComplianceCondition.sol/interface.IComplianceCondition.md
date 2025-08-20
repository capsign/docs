# IComplianceCondition
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Tokenization/assets/interfaces/IComplianceCondition.sol)

Interface for compliance condition contracts


## Functions
### validateTransfer


```solidity
function validateTransfer(address from, address to, uint256 lotId, uint256 amount) external view returns (bool);
```

### processTransfer


```solidity
function processTransfer(address from, address to, uint256 lotId, uint256 amount) external;
```

### processIssuance


```solidity
function processIssuance(address to, uint256 lotId, uint256 amount, bytes calldata data) external;
```

### processRedemption


```solidity
function processRedemption(address from, uint256 lotId, uint256 amount) external;
```

### migrateLotState


```solidity
function migrateLotState(bytes32 oldLotId, bytes32 newLotId) external;
```

### conditionName


```solidity
function conditionName() external view returns (string memory);
```

