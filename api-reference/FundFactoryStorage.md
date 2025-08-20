# FundFactoryStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/Funds/factory/storage/FundFactoryStorage.sol)

Simplified storage layout for FundFactory diamond using unified FacetRegistry

*Registry functionality moved to unified FacetRegistry - this only tracks deployments*


## State Variables
### FUND_FACTORY_STORAGE_POSITION

```solidity
bytes32 constant FUND_FACTORY_STORAGE_POSITION = keccak256("capsign.storage.fund_factory");
```


## Functions
### layout


```solidity
function layout() internal pure returns (Layout storage l);
```

### addFund


```solidity
function addFund(address fundAddress, string memory name, string memory symbol, address entity, string memory fundType)
    internal;
```

### getFundInfo


```solidity
function getFundInfo(address fundAddress) internal view returns (FundInfo memory);
```

### getFundsByType


```solidity
function getFundsByType(string memory fundType) internal view returns (address[] memory);
```

### getFundCountByType


```solidity
function getFundCountByType(string memory fundType) internal view returns (uint256);
```

## Structs
### FundInfo

```solidity
struct FundInfo {
    string name;
    string symbol;
    address entity;
    uint256 deployedAt;
    IUniversalFund.FundStatus status;
    string fundType;
}
```

### GovernanceConfig

```solidity
struct GovernanceConfig {
    uint256 votingDelay;
    uint256 votingPeriod;
    uint256 proposalThreshold;
    uint256 quorumNumerator;
    uint256 timelockDelay;
    address guardian;
}
```

### Layout

```solidity
struct Layout {
    mapping(address => FundInfo) fundInfo;
    mapping(address => address) fundGovernance;
    mapping(address => address) fundTimelock;
    mapping(address => GovernanceConfig) governanceConfigs;
    mapping(string => address[]) fundsByType;
    mapping(string => uint256) fundCountsByType;
    address capitalAccountsImplementation;
    address distributionEngineImplementation;
    address fundUnitImplementation;
    address governanceImplementation;
    address timelockImplementation;
    address diamondFactory;
    bool initialized;
    uint256 version;
    uint256[50] __gap;
}
```

