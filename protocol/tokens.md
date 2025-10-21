# Token Architecture

CapSign tokens are ERC-7752 securities tokens with lot-based accounting.

## Overview

Features:
- ERC-20 compatibility
- ERC-7752 lot tracking
- Transfer restrictions
- Compliance modules
- Administrative controls

## Diamond Structure

```
TokenDiamond
├── DiamondCutFacet
├── DiamondLoupeFacet
├── AccessControlFacet
├── TokenERC20Facet - ERC-20 functions
├── TokenBalancesFacet - Balance tracking
├── TokenLotsFacet - Lot management (ERC-7752)
├── TokenTransferFacet - Transfer logic
├── TokenComplianceFacet - Compliance checks
├── TokenMetadataFacet - Token metadata
└── TokenAdminFacet - Admin functions
```

## Key Interfaces

### ERC-20

Standard ERC-20 functions:

```solidity
interface IERC20 {
  function balanceOf(address account) external view returns (uint256);
  function transfer(address to, uint256 amount) external returns (bool);
  function approve(address spender, uint256 amount) external returns (bool);
  function transferFrom(address from, address to, uint256 amount) external returns (bool);
}
```

### ERC-7752 Lots

Lot-based accounting:

```solidity
interface ITokenLots {
  // Create lot (issuance)
  function createLot(
    address owner,
    uint256 quantity,
    address paymentCurrency,
    uint256 costBasis,
    uint256 acquisitionDate,
    string memory uri,
    bytes memory data,
    uint256 customId
  ) external returns (bytes32 lotId, uint256 assignedCustomId);
  
  // Transfer lot
  function transfer(
    bytes32 lotId,
    address to,
    uint256 quantity,
    TransferType tType,
    uint256 newCostBasis,
    string memory uri,
    bytes memory data
  ) external returns (bytes32 newLotId);
  
  // Get lot info
  function getLot(bytes32 lotId) external view returns (
    bytes32 parentLotId,
    bool isValid,
    uint256 quantity,
    address paymentCurrency,
    uint256 costBasis,
    uint256 acquisitionDate,
    uint256 lastUpdate,
    TransferType tType,
    string memory uri,
    bytes memory data
  );
  
  // Get user's lots
  function getLotsOf(address account) external view returns (bytes32[] memory);
}
```

### Compliance

Transfer restrictions:

```solidity
interface ITokenCompliance {
  // Add compliance condition
  function addComplianceCondition(
    address condition,
    string memory conditionName
  ) external;
  
  // Validate transfer
  function validateTransfer(
    address from,
    address to,
    bytes32 lotId,
    uint256 quantity
  ) external view returns (bool);
}
```

## Lot Structure

Each lot tracks:

```solidity
struct Lot {
  bytes32 parentLotId;      // Parent lot (for tracking splits)
  uint256 quantity;         // Number of tokens
  address paymentCurrency;  // Currency used (USDC, ETH, etc.)
  uint256 costBasis;        // Cost per token
  uint256 acquisitionDate;  // When acquired
  uint256 lastUpdate;       // Last modification
  bool isValid;             // Is lot active?
  address owner;            // Current owner
  TransferType tType;       // How acquired
  string uri;               // Metadata URI
  bytes data;               // Additional data
}
```

## Transfer Types

```solidity
enum TransferType {
  INTERNAL,     // Internal transfer
  SALE,         // Sale/purchase
  GIFT,         // Gift
  INHERITANCE,  // Inheritance
  INCOME        // Income/compensation
}
```

## Compliance Modules

### Condition Contracts

Separate contracts that check transfers:

```solidity
interface IComplianceCondition {
  function checkTransfer(
    address from,
    address to,
    bytes32 lotId,
    uint256 quantity
  ) external view returns (bool allowed, string memory reason);
}
```

### Built-in Conditions

- **LockupCondition** - Time-based lockup
- **VestingCondition** - Linear vesting
- **Rule144Condition** - SEC Rule 144 holding period
- **ROFRCondition** - Right of first refusal

### Example: Lockup

```solidity
contract LockupCondition is IComplianceCondition {
  mapping(bytes32 => uint256) public lockupExpiry;
  
  function checkTransfer(
    address,
    address,
    bytes32 lotId,
    uint256
  ) external view returns (bool, string memory) {
    if (block.timestamp < lockupExpiry[lotId]) {
      return (false, "Tokens locked");
    }
    return (true, "");
  }
}
```

## Deployment

### Via TokenFactory

```solidity
IToken token = ITokenFactory(TOKEN_FACTORY).createToken({
  name: "Class A Common Stock",
  symbol: "CSA",
  decimals: 0,
  baseURI: "https://capsign.com/tokens/",
  admin: msg.sender
}, facetCuts, MULTI_INIT_FLAG, initData, complianceConditions);
```

## Issuance (Minting)

Create lots for recipients:

```solidity
token.createLot({
  owner: investorAddress,
  quantity: 1000,
  paymentCurrency: USDC_ADDRESS,
  costBasis: 1e6, // $1.00 (6 decimals)
  acquisitionDate: block.timestamp,
  uri: "",
  data: "",
  customId: 0
});
```

## Transfers

### ERC-20 Transfer

Simple transfers (uses oldest lot FIFO):

```solidity
token.transfer(recipient, amount);
```

### ERC-7752 Transfer

Specify which lot:

```solidity
token.transfer({
  lotId: lotId,
  to: recipient,
  quantity: amount,
  tType: TransferType.SALE,
  newCostBasis: 2e6, // $2.00
  uri: "",
  data: ""
});
```

## Storage

### TokenBalancesStorage

```solidity
library TokenBalancesStorage {
  struct Layout {
    mapping(address => uint256) balances;
    uint256 totalSupply;
  }
}
```

### TokenLotsStorage

```solidity
library TokenLotsStorage {
  struct Layout {
    mapping(bytes32 => Lot) lots;
    mapping(address => bytes32[]) userLots;
    mapping(uint256 => bytes32) customIdToLotId;
    uint256 nextCustomId;
  }
}
```

## Events

```solidity
event LotCreated(bytes32 indexed lotId, uint256 indexed customId, address indexed owner, uint256 quantity);
event LotTransferred(bytes32 indexed newLotId, address indexed from, address indexed to, uint256 quantity, ...);
event LotInvalidated(bytes32 indexed lotId);
event ComplianceConditionAdded(address indexed condition, string name);
```

## Security

- Reentrancy protection on all transfers
- Access control on admin functions
- Pausable for emergencies
- Compliance checks before every transfer

## Gas Costs

On Base:
- Token deployment: ~6M gas (~$0.10)
- Create lot: ~200k gas (~$0.002)
- ERC-20 transfer: ~100k gas (~$0.001)
- ERC-7752 transfer: ~300k gas (~$0.003)

## Testing

```solidity
function testCreateLot() public {
  (bytes32 lotId,) = token.createLot({
    owner: alice,
    quantity: 1000,
    paymentCurrency: address(usdc),
    costBasis: 1e6,
    acquisitionDate: block.timestamp,
    uri: "",
    data: "",
    customId: 0
  });
  
  assertEq(token.balanceOf(alice), 1000);
}
```

## Resources

- [ERC-7752 Spec](https://eips.ethereum.org/EIPS/eip-7752)
- [ERC-20 Spec](https://eips.ethereum.org/EIPS/eip-20)
- [Contract Addresses](/reference/contract-addresses.md)
