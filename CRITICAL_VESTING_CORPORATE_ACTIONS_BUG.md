# 🚨 CRITICAL: Vesting Module + Corporate Actions Bug

## Issue Summary
The `VestingComplianceModule` stores vesting amounts as **fixed absolute values** (`totalAmount`), which do not update when corporate actions (stock splits, dividends) occur. This creates a catastrophic bug where vested tokens do not scale with corporate actions.

## Reproduction
1. Create a lot with 8,000,000 tokens
2. Set vesting schedule for 8,000,000 tokens over 4 years
3. After 2 years, 4,000,000 tokens are vested
4. Company does a 2:1 stock split
5. Lot now has 16,000,000 tokens (corporate action facet updates this)
6. **BUG**: Vesting module still thinks only 8,000,000 tokens exist
7. **RESULT**: Only 8M of the 16M tokens can ever vest (50% loss!)

## Affected Code
`VestingComplianceModule.sol` lines 28, 241-242, 347:
```solidity
uint256 totalAmount; // FIXED VALUE - never updated!
uint256 vestedAmount = getVestedAmount(lotId); // Uses totalAmount
uint256 available = vested - schedule.releasedAmount; // Wrong after corporate action!
```

## Impact
- **Stock Splits**: Unvested tokens remain locked forever
- **Stock Dividends**: Dividend tokens never vest
- **Reverse Splits**: May allow over-vesting (security risk)

## Proposed Solutions

### Option 1: Percentage-Based Vesting (RECOMMENDED)
Store vesting as a percentage (0-10000 basis points = 0-100%) instead of absolute amount.

**Pros:**
- Automatically scales with corporate actions
- No hook/update mechanism needed
- Clean separation of concerns

**Cons:**
- Requires refactor of vesting module
- Need to migrate existing vesting schedules

### Option 2: Corporate Action Hooks
Add a hook in `TokenCorporateActionsFacet` to update vesting amounts.

**Pros:**
- Simpler migration
- Keeps absolute amounts

**Cons:**
- Tight coupling between facets
- Complex hook logic
- Gas intensive for many vesting schedules

### Option 3: Dynamic Lot Query (BEST SHORT-TERM)
Query current lot quantity dynamically and calculate vesting percentage on-the-fly.

**Implementation:**
```solidity
import {IToken} from "../../interfaces/token/IToken.sol";

function getVestedAmount(bytes32 lotId) public view returns (uint256) {
    VestingSchedule storage schedule = schedules[lotId];
    if (schedule.totalAmount == 0 || schedule.revoked) return 0;

    // Get current lot quantity (accounts for corporate actions)
    IToken.Lot memory lot = IToken(tokenDiamond).getLot(lotId);
    
    // Calculate vesting percentage
    uint256 vestingBps = (schedule.totalAmount * 10000) / schedule.originalLotQuantity;
    
    // Apply percentage to current lot quantity
    uint256 adjustedTotalAmount = (lot.quantity * vestingBps) / 10000;
    
    // Calculate vested based on time
    uint256 currentTime = block.timestamp;
    if (currentTime < schedule.startTime + schedule.cliffDuration) return 0;
    if (currentTime >= schedule.startTime + schedule.duration) return adjustedTotalAmount;
    
    uint256 elapsed = currentTime - schedule.startTime;
    return (adjustedTotalAmount * elapsed) / schedule.duration;
}
```

**Pros:**
- Correctly handles all corporate actions
- No migration needed (add `originalLotQuantity` field)
- Clean and auditable

**Cons:**
- Requires IToken interface import
- Slightly more gas per check
- Need to store original lot quantity

## Recommended Action
Implement **Option 3** immediately:

1. Add `originalLotQuantity` field to `VestingSchedule` struct
2. Store lot quantity when vesting schedule is created
3. Query current lot quantity in `getVestedAmount()`
4. Calculate vesting percentage and apply to current quantity
5. Add comprehensive tests for splits, dividends, reverse splits

## Testing Requirements
- [ ] 2:1 stock split after 50% vesting
- [ ] 10% stock dividend after 25% vesting  
- [ ] 1:2 reverse split after 75% vesting
- [ ] Multiple corporate actions
- [ ] Edge case: Corporate action before cliff

## Priority
**CRITICAL** - This breaks a core feature (vesting) and causes permanent token loss.

## Status
- **Discovered**: 2025-12-02
- **Severity**: Critical
- **Assigned**: Protocol team
- **Target Fix**: Immediate




