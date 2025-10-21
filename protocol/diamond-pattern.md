# Diamond Pattern (EIP-2535)

CapSign uses the Diamond Pattern for all smart contracts. This guide explains why and how.

## What is the Diamond Pattern?

The Diamond Pattern (EIP-2535) is a modular smart contract architecture that:

- Splits functionality into separate contracts called **facets**
- Uses a single proxy contract called a **diamond**
- Delegates calls to the appropriate facet
- Allows unlimited contract size
- Enables upgrades without changing the address

## Why Use Diamonds?

### 1. No Size Limit

Standard contracts have a 24KB limit. Diamonds have no limit.

**Example:**
- Token contract needs: ERC-20, lots, compliance, admin, metadata
- Total: ~40KB
- **Solution:** Split into 5 facets, each <24KB

### 2. Upgradeability

Add, replace, or remove features without redeploying:

```solidity
// Add new feature
diamond.diamondCut([{
  facet: newFacet,
  action: Add,
  selectors: [selector1, selector2, ...]
}]);

// Replace existing feature
diamond.diamondCut([{
  facet: updatedFacet,
  action: Replace,
  selectors: [selector1, selector2, ...]
}]);
```

### 3. Gas Efficiency

Facets deployed once, reused across all diamonds:

- Deploy `TokenERC20Facet` once
- Every token diamond uses the same facet
- ~90% gas savings on deployment

### 4. Modularity

Features organized logically:

- `TokenERC20Facet` - ERC-20 functions
- `TokenLotsFacet` - Lot management
- `TokenAdminFacet` - Admin functions

Easy to understand and maintain.

## Diamond Architecture

### Components

**Diamond (Proxy):**
- Single address
- Delegates to facets
- Stores facet addresses
- Manages upgrades

**Facets (Implementation):**
- Contain actual logic
- Reusable across diamonds
- No storage (use diamond storage)

**Diamond Storage:**
- State stored in diamond
- Each facet has unique storage slot
- No storage collisions

### Call Flow

```
User → Diamond → Facet → Execute → Return → Diamond → User
```

1. User calls function on diamond
2. Diamond looks up which facet handles it
3. Diamond delegates to facet
4. Facet executes using diamond's storage
5. Result returned to user

## CapSign Diamonds

### Wallet Diamond

```
WalletDiamond
├── DiamondCutFacet (upgrades)
├── DiamondLoupeFacet (introspection)
├── AccessControlFacet (permissions)
├── WalletCoreFacet (ERC-4337)
├── WalletSignatureFacet (ERC-1271)
├── WalletDocumentsFacet (documents)
└── WalletIdentityFacet (attestations)
```

### Token Diamond

```
TokenDiamond
├── DiamondCutFacet
├── DiamondLoupeFacet
├── AccessControlFacet
├── TokenERC20Facet (ERC-20)
├── TokenLotsFacet (ERC-7752)
├── TokenTransferFacet (transfers)
├── TokenComplianceFacet (compliance)
├── TokenMetadataFacet (metadata)
└── TokenAdminFacet (admin)
```

### Offering Diamond

```
OfferingDiamond
├── DiamondCutFacet
├── DiamondLoupeFacet
├── AccessControlFacet
├── OfferingCoreFacet (core logic)
├── OfferingComplianceFacet (compliance)
└── OfferingDocumentsFacet (documents)
```

## Diamond Storage

### Why Diamond Storage?

Standard Solidity storage causes collisions when facets share a diamond.

**Solution:** Each facet uses a unique storage slot.

### Implementation

```solidity
library TokenERC20Storage {
  // Unique storage location
  bytes32 constant STORAGE_SLOT = keccak256("token.erc20.storage");
  
  struct Layout {
    mapping(address => uint256) balances;
    mapping(address => mapping(address => uint256)) allowances;
    uint256 totalSupply;
  }
  
  function layout() internal pure returns (Layout storage l) {
    bytes32 slot = STORAGE_SLOT;
    assembly {
      l.slot := slot
    }
  }
}
```

### Usage in Facets

```solidity
contract TokenERC20Facet {
  function balanceOf(address account) external view returns (uint256) {
    return TokenERC20Storage.layout().balances[account];
  }
  
  function transfer(address to, uint256 amount) external returns (bool) {
    TokenERC20Storage.Layout storage s = TokenERC20Storage.layout();
    s.balances[msg.sender] -= amount;
    s.balances[to] += amount;
    return true;
  }
}
```

## Upgrading Diamonds

### DiamondCut

Add, replace, or remove facets:

```solidity
enum FacetCutAction { Add, Replace, Remove }

struct FacetCut {
  address facet;
  FacetCutAction action;
  bytes4[] selectors;
}

function diamondCut(
  FacetCut[] memory cuts,
  address init,
  bytes memory initData
) external;
```

### Example: Adding Feature

```solidity
// Deploy new facet
TokenVotingFacet votingFacet = new TokenVotingFacet();

// Add to diamond
diamond.diamondCut([{
  facet: address(votingFacet),
  action: FacetCutAction.Add,
  selectors: [
    TokenVotingFacet.vote.selector,
    TokenVotingFacet.delegate.selector,
    TokenVotingFacet.getVotes.selector
  ]
}], address(0), "");
```

## Best Practices

### Facet Design

- **Single responsibility** - Each facet does one thing
- **No storage** - Use diamond storage
- **Internal functions** - Use base contracts for shared logic
- **Events** - Emit events for state changes

### Security

- **Access control** - Protect sensitive functions
- **Reentrancy guards** - Prevent reentrancy attacks
- **Pause mechanisms** - Emergency stop functionality
- **Upgrade safeguards** - Require multi-sig for upgrades

### Gas Optimization

- **Singleton facets** - Deploy facets once, reuse everywhere
- **Efficient selectors** - Group related functions in same facet
- **Minimal delegatecalls** - Keep call paths short

## Limitations

### Selector Conflicts

Each function selector must be unique across all facets:

```solidity
// ❌ Both facets can't have transfer()
FacetA.transfer()
FacetB.transfer()

// ✅ Use different names or signatures
FacetA.transferTokens()
FacetB.transferOwnership()
```

### Complexity

Diamonds are more complex than standard contracts:

- More files to manage
- Deployment more involved
- Testing requires more setup

**Worth it for large, upgradeable systems.**

## Resources

- [EIP-2535 Spec](https://eips.ethereum.org/EIPS/eip-2535)
- [Diamond Reference Implementation](https://github.com/mudgen/diamond)
- [CapSign Protocol Contracts](https://github.com/capsign/protocol)

## See Also

- [Wallets](wallets.md) - Wallet diamond architecture
- [Tokens](tokens.md) - Token diamond architecture
- [Offerings](offerings.md) - Offering diamond architecture
