# Compliance Modules Architecture

## Overview

CapSign's compliance system uses a **shared singleton pattern** where compliance modules are deployed once and referenced by multiple tokens.

## Architecture

### 1. Shared Modules (Not Cloned)

```
┌─────────────────────────────────────┐
│   HoldingPeriodModule (Singleton)   │
│   --------------------------------   │
│   mapping(token => lotId => holder) │
│   - Stores data for ALL tokens      │
│   - Deployed once at:               │
│     0x1234...abcd                   │
└─────────────────────────────────────┘
           ▲           ▲
           │           │
    ┌──────┘           └──────┐
    │                         │
┌───┴────┐              ┌─────┴───┐
│ Token A│              │ Token B │
│ ────── │              │ ─────── │
│ modules│              │ modules │
│ [0x123]│              │ [0x123] │
└────────┘              └─────────┘
```

**Key Points:**
- ✅ One module contract serves multiple tokens
- ✅ Module stores data using `mapping(address token => ...)`
- ✅ Each token just stores module **addresses** in an array
- ✅ Gas efficient - no cloning required

### 2. Data Storage Pattern

```solidity
// HoldingPeriodModule.sol
contract HoldingPeriodModule {
    // Nested mappings: token => lotId => holder => data
    mapping(address => mapping(bytes32 => mapping(address => uint256))) public acquisitionDate;
    mapping(address => uint256) public holdingPeriod;
}
```

**Examples:**
```solidity
// Token 0xAAA with lot 0x111 for holder 0xBBB
acquisitionDate[0xAAA][0x111][0xBBB] = 1704067200;

// Token 0xCCC with lot 0x222 for holder 0xDDD
acquisitionDate[0xCCC][0x222][0xDDD] = 1735689600;
```

### 3. Token References Modules

```solidity
// TokenComplianceFacet.sol (in each token diamond)
contract TokenComplianceFacet {
    struct TokenComplianceStorage {
        address[] globalModules;  // Array of module addresses
        mapping(address => bool) isModuleRegistered;
        mapping(bytes32 => address[]) lotModules;  // Lot-specific modules
    }
}
```

**Example:**
```solidity
// Token 0xAAA might have:
globalModules = [
    0x1111...  // HoldingPeriodModule
    0x2222...  // VolumeLimitModule
    0x3333...  // AffiliateStatusModule
]
```

## Initialization Patterns

### Option 1: Add Modules After Creation

```solidity
// 1. Create token with no modules
tokenFactory.createToken(
    ...,
    new address[](0),  // Empty modules
    new string[](0)    // Empty names
);

// 2. Later, admin adds compliance modules
token.addComplianceModule(0x1111..., "HoldingPeriod");
token.addComplianceModule(0x2222..., "VolumeLimit");
```

### Option 2: Add Modules During Creation ✨

```solidity
// Prepare module arrays
address[] memory modules = new address[](2);
modules[0] = 0x1111...;  // HoldingPeriodModule
modules[1] = 0x2222...;  // VolumeLimitModule

string[] memory names = new string[](2);
names[0] = "HoldingPeriod";
names[1] = "VolumeLimit";

// Pass to TokenCompliance_init
initData = abi.encodeCall(
    TokenComplianceFacet.TokenCompliance_init,
    (admin, modules, names)  // Includes modules!
);
```

## Transfer Validation Flow

```
User initiates transfer
        │
        ▼
TokenTransferFacet.transfer()
        │
        ▼
TokenComplianceFacet.validateTransferCompliance()
        │
        ├─► Loop through globalModules[]
        │   │
        │   ├─► HoldingPeriodModule.validateTransfer() ─► staticcall
        │   │   └─► Checks: acquisitionDate[token][lot][holder] + holdingPeriod < now
        │   │
        │   ├─► VolumeLimitModule.validateTransfer() ─► staticcall
        │   │   └─► Checks: currentVolume + quantity <= limit
        │   │
        │   └─► AffiliateStatusModule.validateTransfer() ─► staticcall
        │       └─► Checks: if affiliate, apply volume limits
        │
        ├─► If lotHasSpecificModules[lotId]:
        │   └─► Also loop through lotModules[lotId][]
        │
        └─► Return true if ALL modules pass
```

## Module Configuration Flow

```
Admin sets up Rule 144 compliance:

1. Admin calls HoldingPeriodModule.setHoldingPeriod(tokenAddr, 365 days)
   ├─► Stores: holdingPeriod[tokenAddr] = 31536000

2. Admin calls VolumeLimitModule.setVolumeLimit(tokenAddr, 100, 7 days)
   ├─► Stores: limitBps[tokenAddr] = 100 (1%)
   └─► Stores: windowSeconds[tokenAddr] = 604800

3. When lot is created, system calls HoldingPeriodModule.setAcquisitionDate()
   ├─► Stores: acquisitionDate[tokenAddr][lotId][holder] = block.timestamp

4. Future transfers are validated against these configs
```

## Benefits of This Architecture

### 1. Gas Efficiency
- ✅ Deploy once, use everywhere
- ✅ No cloning overhead
- ✅ Shared bytecode

### 2. Upgradeability
- ✅ Deploy new module version
- ✅ Tokens can migrate when ready
- ✅ No forced upgrades

### 3. Composability
- ✅ Mix and match modules per token
- ✅ Legal presets = arrays of module addresses
- ✅ Easy to add/remove

### 4. Flexibility
- ✅ Global modules (all transfers)
- ✅ Lot-specific modules (specific lots only)
- ✅ Per-token configuration within shared module

## Legal Presets

Presets are just predefined arrays of module addresses + configs:

```typescript
const RULE_144_PRESET = {
  name: "US Rule 144",
  modules: [
    {
      address: "0x1111...",  // HoldingPeriodModule
      name: "HoldingPeriod",
      config: { period: 365 * 24 * 60 * 60 }  // 1 year
    },
    {
      address: "0x2222...",  // VolumeLimitModule
      name: "VolumeLimit",
      config: { limitBps: 100, window: 7 * 24 * 60 * 60 }  // 1%, 7 days
    },
    {
      address: "0x3333...",  // AffiliateStatusModule
      name: "AffiliateStatus",
      config: {}
    }
  ]
};
```

## Comparison: Shared vs Cloned

| Aspect | Shared Singleton | Cloned per Token |
|--------|-----------------|------------------|
| **Deployment Gas** | Deploy once | Deploy per token |
| **Storage** | `mapping(token => data)` | Direct state vars |
| **Upgrades** | Deploy new, tokens migrate | Must upgrade each clone |
| **Code Size** | Smaller (one copy) | Larger (N copies) |
| **Complexity** | Need token param | Simpler interface |
| **Our Choice** | ✅ **YES** | ❌ No |

## Examples

### Rule 144 Setup

```solidity
// 1. Modules already deployed (shared singletons)
address holdingPeriodModule = 0x1111...;
address volumeLimitModule = 0x2222...;
address affiliateModule = 0x3333...;

// 2. Create token WITH modules
tokenFactory.createToken(
    ...,
    [holdingPeriodModule, volumeLimitModule, affiliateModule],
    ["HoldingPeriod", "VolumeLimit", "AffiliateStatus"]
);

// 3. Configure modules for this specific token
HoldingPeriodModule(holdingPeriodModule).setHoldingPeriod(
    tokenAddress, 
    365 days  // Rule 144 holding period
);

VolumeLimitModule(volumeLimitModule).setVolumeLimit(
    tokenAddress,
    100,      // 1% of outstanding shares
    7 days    // Rolling 7-day window
);

// 4. When lots are created, acquisition dates are set automatically
// 5. When transfers happen, all modules validate
```

## Summary

✅ **Modules are shared singletons** - deployed once, used by many tokens  
✅ **Tokens store module addresses** - lightweight array in diamond storage  
✅ **Modules store data per-token** - using `mapping(address token => ...)`  
✅ **Configuration is per-token** - same module, different configs  
✅ **Init with modules** - `TokenCompliance_init(admin, modules[], names[])` supports preset application at creation time

This architecture provides maximum flexibility, gas efficiency, and composability! 🎯

