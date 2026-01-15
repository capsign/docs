# Compliance Module Architecture Analysis

## Status Update: December 23, 2025

✅ **VESTING + CORPORATE ACTIONS BUG: FIXED**

The critical vesting bug has been **completely resolved**. The `VestingComplianceModule` now uses **percentage-based vesting** (basis points) that automatically scales with stock splits, reverse splits, and dividends.

**Test Results**: All 29 tests pass, including:
- ✅ `test_VestingScalesWithStockSplit` - Verified 2:1 split compatibility
- ✅ `test_VestingScalesWithReverseSplit` - Verified 1:2 reverse split compatibility  
- ✅ `test_VestingScalesWithDividend` - Verified stock dividend compatibility

---

## Historical Context: Original Issue (NOW FIXED)

### 1. **~~CRITICAL: Vesting + Corporate Actions Bug~~** ✅ RESOLVED
**~~Problem~~**: ~~`VestingComplianceModule.totalAmount` is a fixed value that doesn't update with stock splits/dividends.~~

**Solution Implemented**: Uses `totalBasisPoints` (percentage) instead of fixed amounts. Automatically scales with all corporate actions.

**~~Impact~~ (Historical - No Longer Applicable)**: 
- ~~2:1 split → 50% of tokens permanently locked~~
- ~~Reverse split → potential over-vesting security vulnerability~~
- ~~Stock dividend → dividend shares never vest~~

### 2. **Access Control with Singleton Modules**
**Problem**: Singleton compliance modules use `AccessControl` with `protected` modifier, but have no way to know which token/user is authorized.

**Current Flow**:
```
User (0xABC) → VestingModule.createVestingSchedule()
                ↓
            protected modifier checks: "Is 0xABC authorized?"
                ↓
            AccessControl → Authority: ??? (module has no authority set)
                ↓
            REVERT: AccessControl_CallerIsNotAuthorized
```

**Why This Happens**:
- Modules are singletons (one contract for all tokens)
- Each token has its own admin
- Module doesn't know about token-level permissions
- No authority/access manager configured for module

### 3. **Lockup Module - Lot ID Tracking**
**Current Design**: Lockup is tied to `lotId`, not to holder.

**Issue**: When a lot is transferred and split into new lots:
- Original lot's lockup doesn't transfer to new lots
- This may be intentional (lockup transfers with the lot)
- But creates UX confusion for partial transfers

## Root Cause: Design Mismatch

The fundamental issue is **architectural mismatch** between:
1. **Singleton modules** (one instance serves all tokens)
2. **AccessControl** (designed for per-contract permissions)
3. **Token-specific admin** (each token has different admins)

## Proposed Solutions

### Solution A: Remove AccessControl from Modules (RECOMMENDED)

**Change**: Make compliance modules "permissionless" - they trust the calling token to enforce auth.

**Implementation**:
```solidity
// Before:
function createVestingSchedule(...) external protected { ... }

// After:
function createVestingSchedule(...) external {
    // No access control - caller must be the token diamond
    // Token diamond enforces admin check before calling module
}
```

**Pros**:
- ✅ Simple, clean architecture
- ✅ No wrapper functions needed
- ✅ Modules are pure logic, tokens handle auth
- ✅ Follows separation of concerns

**Cons**:
- ⚠️ Any contract can call module functions
- ⚠️ Relies on tokens to validate before calling
- ⚠️ But modules only *enforce* rules, they don't hold critical state

**Security Analysis**:
- Modules are stateless validators (compliance rules)
- Even if a malicious contract calls module, it can't:
  - Steal tokens (modules don't hold tokens)
  - Bypass compliance (modules are view-only in validateTransfer)
  - Affect other tokens (each token's data is isolated by lotId/tokenAddress)
- The worst a malicious caller can do is set lockup/vesting on their own lots

### Solution B: Wrapper Functions in TokenComplianceFacet

**Change**: Add wrapper functions that proxy to modules.

**Implementation**:
```solidity
// In TokenComplianceFacet
function setLotVesting(...) external protected {
    // 'protected' checks if caller is token admin
    IVestingModule(VESTING_MODULE).createVestingSchedule(...);
}
```

**Pros**:
- ✅ Maintains access control
- ✅ Clear authorization flow
- ✅ Explicit token → module relationship

**Cons**:
- ❌ Requires wrapper for every module function
- ❌ More boilerplate code
- ❌ Modules still have unused AccessControl
- ❌ Doesn't fix the fundamental singleton + AccessControl mismatch

### Solution C: Module Authority Configuration

**Change**: Set each token diamond as the authority for its modules.

**Issue**: Modules are singletons serving multiple tokens. Can't set one authority.

**Verdict**: ❌ Not viable for singleton architecture

## Recommended Approach: Hybrid Solution

### Phase 1: Fix Critical Bugs (Immediate)
1. **Remove `protected` from singleton modules** (VestingComplianceModule, LockupComplianceModule)
2. **Add wrapper functions in TokenComplianceFacet** for admin operations
3. **Fix vesting corporate actions bug** (see below)

### Phase 2: Vesting Corporate Actions Fix ✅ COMPLETE

**✅ IMPLEMENTED: Percentage-Based Vesting**

Vesting now uses **basis points** (0-10000 = 0-100%) instead of absolute amounts.

```solidity
// CURRENT IMPLEMENTATION (packages/tokens/src/compliance/modules/VestingComplianceModule.sol)
struct VestingSchedule {
    address tokenHolder;
    bool revocable;
    bool revoked;
    uint256 totalBasisPoints; // 10000 = 100% of lot
    uint256 releasedBasisPoints; // How much has been released (in bp)
    uint256 startTime;
    uint256 duration;
    uint256 cliffDuration;
}

// Lines 266-281: Corporate action compatible vesting calculation
function getVestedAmount(bytes32 lotId) public view returns (uint256) {
    VestingSchedule storage schedule = schedules[lotId];
    if (schedule.totalBasisPoints == 0 || schedule.revoked) return 0;
    
    // Get current lot quantity (in raw units) - auto-adjusts for corporate actions
    uint256 lotQuantityRaw = _getLotQuantityRaw(lotId);
    
    // Calculate time-based vested percentage
    uint256 vestedBasisPoints = _calculateVestedBasisPoints(schedule);
    
    // Apply both percentages: (lot qty) * (vesting %) * (time vested %)
    return (lotQuantityRaw * schedule.totalBasisPoints * vestedBasisPoints) / (10_000 * 10_000);
}
```

**Achieved Benefits**:
- ✅ Automatically scales with stock splits
- ✅ Automatically scales with stock dividends
- ✅ No hooks or callbacks needed
- ✅ Mathematically sound
- ✅ Clean separation of concerns
- ✅ Fully tested with 29 passing tests

### Phase 3: Cleanup (Optional)
- Remove unused AccessControl from modules
- Document that modules are "permissionless validators"
- Add integration tests for corporate actions + vesting

## Similar Bugs to Check

### 1. HoldingPeriodModule
**Status**: ✅ SAFE - Uses `acquisitionDate` (timestamp), not quantity
Corporate actions don't affect time-based restrictions.

### 2. VolumeLimitModule
**Status**: ⚠️ NEEDS REVIEW
```solidity
uint256 currentVolume; // Tracks transferred amount
uint256 limitBps; // 1% = 100 basis points of balance
```

**Question**: Is `currentVolume` updated when corporate actions occur?
- If NO → After 2:1 split, 1% limit applies to double the shares but currentVolume is stale
- Should likely be reset or adjusted during corporate actions

### 3. AffiliateStatusModule
**Status**: ✅ SAFE - Boolean flag, no quantities involved

### 4. LockupComplianceModule
**Status**: ✅ SAFE - Time-based, no quantities

## Implementation Priority

### P0 (Critical - Completed ✅):
1. ✅ Document the issue (this file)
2. ✅ ~~Remove `protected` from singleton module admin functions~~ - Modules use caller validation
3. ✅ Add wrapper functions to `TokenComplianceFacet` - See lines 287-424
4. ⏸️ Deploy updated modules and facets - Ready for deployment

### P1 (High - Completed ✅):
1. ✅ Refactor `VestingComplianceModule` to use percentage-based vesting - DONE
2. ✅ Add `getLotQuantity()` helper to `TokenLotsFacet` - Available via `_getLotQuantityRaw()`
3. ✅ Write integration tests for vesting + corporate actions - 29 tests passing
4. ⚠️ Review and fix `VolumeLimitModule` if needed - **STILL NEEDS REVIEW**

### P2 (Medium - Future):
1. Remove unused AccessControl from modules (breaking change)
2. Add comprehensive compliance module documentation
3. Create admin UI for managing compliance via wrappers

## Testing Requirements

### Test Coverage - ✅ COMPLETE:
1. ✅ Vesting + 2:1 stock split - `test_VestingScalesWithStockSplit()`
2. ✅ Vesting + reverse split (1:2) - `test_VestingScalesWithReverseSplit()`
3. ✅ Vesting + stock dividend - `test_VestingScalesWithDividend()`
4. ✅ Vesting + cliff periods - `test_GetVestedAmount_BeforeCliff()`, `test_GetVestedAmount_AfterCliff()`
5. ✅ Lockup + transfer - LockupComplianceModule (time-based, safe)
6. ⚠️ Volume limit + stock split - **NEEDS TEST COVERAGE**

### Test Results (All Passing):
```bash
$ forge test --match-contract "VestingComplianceModule"
Ran 29 tests for VestingComplianceModuleTest
[PASS] test_VestingScalesWithStockSplit() (gas: 218902)
[PASS] test_VestingScalesWithReverseSplit() (gas: 215269)
[PASS] test_VestingScalesWithDividend() (gas: 212278)
... 26 more tests PASS
Suite result: ok. 29 passed; 0 failed; 0 skipped
```

Verified Scenarios:
- ✅ 2:1 split: 1M → 2M tokens, vesting scales proportionally
- ✅ 1:2 reverse split: 1M → 500K tokens, no over-vesting
- ✅ 10% dividend: 1M → 1.1M tokens, vesting scales correctly

## Final Status

**✅ VESTING BUG: RESOLVED AND PRODUCTION-READY**

The critical vesting + corporate actions bug has been **completely fixed** with percentage-based vesting:

**Completed**:
- ✅ Phase 1: Access control via wrapper functions in `TokenComplianceFacet`
- ✅ Phase 2: Percentage-based vesting implementation with `totalBasisPoints`
- ✅ Phase 2: Comprehensive test coverage (29 passing tests)
- ✅ Security: Caller validation prevents cross-token attacks

**Ready for Production**:
- ✅ Vesting now safe to use with stock splits
- ✅ Vesting now safe to use with reverse splits
- ✅ Vesting now safe to use with stock dividends
- ✅ No breaking changes to existing interface

**Remaining Work**:
- ⚠️ **VolumeLimitModule** needs review for corporate action compatibility
- 📋 Update interface documentation to remove vesting warnings
- 🚀 Deploy to mainnet when ready




