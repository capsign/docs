# CMXStorage
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/CMX/storage/CMXStorage.sol)

Shared storage library for CMX diamond

*Consolidates all state variables for the CMX token diamond*


## State Variables
### CMX_STORAGE_POSITION

```solidity
bytes32 constant CMX_STORAGE_POSITION = keccak256("capsign.storage.cmx");
```


### MAX_SUPPLY

```solidity
uint256 constant MAX_SUPPLY = 1_000_000_000 ether;
```


## Functions
### layout


```solidity
function layout() internal pure returns (Layout storage l);
```

### initialize


```solidity
function initialize(string memory _name, string memory _symbol, uint8 _decimals, address _lzEndpoint, address _owner)
    internal;
```

### mint


```solidity
function mint(address to, uint256 amount) internal;
```

### burn


```solidity
function burn(address from, uint256 amount) internal;
```

### transfer


```solidity
function transfer(address from, address to, uint256 amount) internal;
```

### approve


```solidity
function approve(address owner, address spender, uint256 amount) internal;
```

### setPaymasterApproval


```solidity
function setPaymasterApproval(address paymaster, bool approved) internal;
```

### isApprovedPaymaster


```solidity
function isApprovedPaymaster(address paymaster) internal view returns (bool);
```

### setPaymasterAllowance


```solidity
function setPaymasterAllowance(address owner, address paymaster, uint256 amount) internal;
```

### getPaymasterAllowance


```solidity
function getPaymasterAllowance(address owner, address paymaster) internal view returns (uint256);
```

### setMinter


```solidity
function setMinter(address minter, bool authorized) internal;
```

### isMinter


```solidity
function isMinter(address account) internal view returns (bool);
```

### setPaused


```solidity
function setPaused(bool _mintingPaused, bool _transfersPaused) internal;
```

### getMaxSupply


```solidity
function getMaxSupply() internal pure returns (uint256);
```

### isInitialized


```solidity
function isInitialized() internal view returns (bool);
```

### getOwner


```solidity
function getOwner() internal view returns (address);
```

### getLzEndpoint


```solidity
function getLzEndpoint() internal view returns (address);
```

### getSourceChain


```solidity
function getSourceChain() internal view returns (bool);
```

### setSourceChain


```solidity
function setSourceChain(bool _isSourceChain) internal;
```

### setOwner


```solidity
function setOwner(address newOwner) internal;
```

## Structs
### Lot

```solidity
struct Lot {
    bytes32 parentLotId;
    uint256 quantity;
    address paymentCurrency;
    uint256 costBasis;
    uint256 acquisitionDate;
    uint256 lastUpdate;
    bool isValid;
    address owner;
    TransferType tType;
    string uri;
    bytes data;
}
```

### Layout

```solidity
struct Layout {
    mapping(address => uint256) balances;
    mapping(address => mapping(address => uint256)) allowances;
    uint256 totalSupply;
    string name;
    string symbol;
    uint8 decimals;
    bool isSourceChain;
    bool initialized;
    address lzEndpoint;
    address owner;
    mapping(address => bool) approvedPaymasters;
    mapping(address => mapping(address => uint256)) paymasterAllowances;
    mapping(address => address) delegates;
    mapping(address => uint256) votingPower;
    uint256 governanceReserved1;
    uint256 governanceReserved2;
    mapping(address => bool) minters;
    bool mintingPaused;
    bool transfersPaused;
    TransferStrategy transferStrategy;
    mapping(bytes32 => Lot) lots;
    mapping(address => bytes32[]) ownerLots;
    uint256 nextCustomId;
    mapping(uint256 => bytes32) customIdToLotId;
    mapping(bytes32 => uint256) lotIdToCustomId;
    mapping(bytes32 => mapping(address => uint256)) lotApprovals;
    mapping(address => mapping(address => bool)) operatorApprovals;
}
```

## Enums
### TransferType

```solidity
enum TransferType {
    INTERNAL,
    SALE,
    GIFT,
    INHERITANCE,
    REWARD
}
```

### TransferStrategy

```solidity
enum TransferStrategy {
    FIFO,
    LIFO,
    HIGHEST_COST_BASIS,
    LOWEST_COST_BASIS
}
```

