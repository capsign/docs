# Secure Compliance Module Solution

## Problem Recap
- Singleton modules have **shared global state** (schedules mapping)
- LotIds **don't include token address** → predictable and reusable across tokens
- Removing access control → attacker can set vesting on ANY lot from ANY token
- Current access control doesn't work with singleton architecture

## Attack Vector (Without Proper Security)
```solidity
// Attacker calculates victim's lotId (public info)
bytes32 victimLotId = keccak256(abi.encodePacked(owner, customId, timestamp, block));

// Attacker calls singleton module directly
VestingModule.createVestingSchedule(
    victimLotId,              // Victim's lot from ANY token
    attackerAddress,          // Attacker as beneficiary
    victimLot.quantity,       // Lock all tokens
    block.timestamp + 1000 years, // Far future
    1000 years,
    0,
    false
);

// Result: Victim's tokens permanently locked, attacker gets vested tokens
```

## Secure Solution: Caller Validation

**Key Insight**: The module should only accept calls **from the token diamond that owns the lot**.

### Implementation:

```solidity
// In VestingComplianceModule.sol

// Add token-to-lotId tracking
mapping(bytes32 => address) public lotToToken; // lotId => token diamond address

function createVestingSchedule(
    bytes32 lotId,
    address tokenHolder,
    uint256 totalAmount,
    uint256 startTime,
    uint256 duration,
    uint256 cliffDuration,
    bool revocable
) external {
    // SECURITY: Only the token diamond that created this lot can manage it
    if (lotToToken[lotId] == address(0)) {
        // First time this lot is registered - caller must be the token
        lotToToken[lotId] = msg.sender;
    } else {
        // Lot already registered - caller must be the registered token
        if (msg.sender != lotToToken[lotId]) {
            revert Vesting_UnauthorizedCaller();
        }
    }
    
    // Validation...
    if (lotId == bytes32(0)) revert Vesting_InvalidLotId();
    // ... rest of validation
    
    // Create schedule
    schedules[lotId] = VestingSchedule({...});
    
    emit VestingScheduleCreated(...);
}
```

### How This Works:

1. **First Call (During lot creation)**:
   - Token A calls `createVestingSchedule(lotId123, ...)`
   - Module checks: `lotToToken[lotId123]` is `address(0)` (unregistered)
   - Module registers: `lotToToken[lotId123] = Token A`
   - Schedule created

2. **Attacker Attempt**:
   - Attacker calls `createVestingSchedule(lotId123, ...)`
   - Module checks: `lotToToken[lotId123]` = Token A
   - Module sees: `msg.sender` = Attacker (not Token A)
   - Reverts: `Vesting_UnauthorizedCaller`

3. **Token Diamond Wrapper**:
   ```solidity
   // In TokenComplianceFacet
   function setLotVesting(...) external protected {
       // 'protected' ensures caller is token admin
       // Then token diamond calls module on behalf of admin
       IVestingModule(VESTING_MODULE).createVestingSchedule(...);
   }
   ```

### Security Analysis:

✅ **Prevents Cross-Token Attacks**: Lot can only be managed by the token that first registered it
✅ **Prevents Direct Attacker Calls**: Only registered token can update schedules
✅ **No Collision Risk**: Even if lotIds collide (unlikely), first token to register "owns" it
✅ **Simple**: One mapping, one check
✅ **Gas Efficient**: SLOAD (5k gas) vs complex access control

### Advantages Over Other Solutions:

**vs. AccessControl**:
- ✅ Works with singletons (one module, many tokens)
- ✅ No authority configuration needed
- ✅ Simpler, less gas

**vs. Wrapper-Only**:
- ✅ Defense in depth (module validates caller even if wrapper has bugs)
- ✅ Prevents accidental misuse
- ✅ Clear ownership model

**vs. Per-Token Module Clones**:
- ✅ Simpler deployment
- ✅ Single source of truth for compliance logic
- ✅ Easier upgrades (deploy new singleton, update token references)

## Complete Implementation Plan

### Phase 1: Security Fix (Immediate)

**1. Update Compliance Modules**:
```solidity
// VestingComplianceModule.sol
mapping(bytes32 => address) public lotToToken;

function createVestingSchedule(...) external {
    _validateCaller(lotId);
    // ... rest of logic
}

function _validateCaller(bytes32 lotId) private {
    if (lotToToken[lotId] == address(0)) {
        lotToToken[lotId] = msg.sender; // Register token
    } else if (msg.sender != lotToToken[lotId]) {
        revert Vesting_UnauthorizedCaller();
    }
}
```

**2. Same for LockupComplianceModule**:
```solidity
mapping(bytes32 => address) public lotToToken;

function setLockup(bytes32 lotId, uint256 unlockTime) external {
    _validateCaller(lotId);
    lotUnlockTime[lotId] = unlockTime;
    emit LockupSet(lotId, unlockTime);
}
```

**3. Add Wrapper Functions in TokenComplianceFacet**:
```solidity
function setLotVesting(
    bytes32 lotId,
    address tokenHolder,
    uint256 totalAmount,
    uint256 startTime,
    uint256 duration,
    uint256 cliffDuration,
    bool revocable
) external protected {
    IVestingModule(VESTING_MODULE).createVestingSchedule(
        lotId, tokenHolder, totalAmount, startTime, duration, cliffDuration, revocable
    );
}

function setLotLockup(bytes32 lotId, uint256 unlockTime) external protected {
    ILockupModule(LOCKUP_MODULE).setLockup(lotId, unlockTime);
}

function setLotAffiliate(bytes32 lotId, address holder, bool isAffiliate) external protected {
    IAffiliateModule(AFFILIATE_MODULE).setAffiliateStatus(
        address(this), lotId, holder, isAffiliate
    );
}
```

### Phase 2: Percentage-Based Vesting (Next Sprint)

**1. Update VestingSchedule struct**:
```solidity
struct VestingSchedule {
    address tokenHolder;
    uint256 totalBasisPoints; // 10000 = 100% of lot (NEW)
    uint256 releasedBasisPoints; // Released percentage (NEW)
    uint256 startTime;
    uint256 duration;
    uint256 cliffDuration;
    bool revocable;
    bool revoked;
}
```

**2. Query lot quantity dynamically**:
```solidity
function getVestedAmount(bytes32 lotId) public view returns (uint256) {
    VestingSchedule storage schedule = schedules[lotId];
    if (schedule.totalBasisPoints == 0 || schedule.revoked) return 0;
    
    // Get current lot quantity from token (auto-adjusts for corporate actions)
    address token = lotToToken[lotId];
    uint256 currentLotQuantity = ITokenLots(token).getLot(lotId).quantity;
    
    // Calculate time-based vested percentage
    uint256 vestedBasisPoints = _calculateVestedBasisPoints(schedule);
    
    // Apply to current lot quantity
    return (currentLotQuantity * vestedBasisPoints) / 10000;
}

function _calculateVestedBasisPoints(
    VestingSchedule storage schedule
) private view returns (uint256) {
    uint256 currentTime = block.timestamp;
    
    // Before cliff
    if (currentTime < schedule.startTime + schedule.cliffDuration) {
        return 0;
    }
    
    // After full vesting
    if (currentTime >= schedule.startTime + schedule.duration) {
        return schedule.totalBasisPoints;
    }
    
    // During vesting (linear)
    uint256 timeVested = currentTime - schedule.startTime;
    return (schedule.totalBasisPoints * timeVested) / schedule.duration;
}
```

**3. Update validateTransfer**:
```solidity
function validateTransfer(
    address from,
    address to,
    bytes32 lotId,
    uint256 quantity
) external view returns (bool) {
    to; // Silence warning
    VestingSchedule storage schedule = schedules[lotId];
    
    if (schedule.totalBasisPoints == 0) return true; // No vesting
    if (schedule.revoked) return false; // Revoked
    if (from != schedule.tokenHolder) return false; // Wrong holder
    
    // Get current vested amount (auto-adjusts for corporate actions)
    uint256 vestedAmount = getVestedAmount(lotId);
    uint256 releasedAmount = (
        ITokenLots(lotToToken[lotId]).getLot(lotId).quantity 
        * schedule.releasedBasisPoints
    ) / 10000;
    
    uint256 available = vestedAmount - releasedAmount;
    
    return quantity <= available;
}
```

**4. Migration for existing schedules**:
```solidity
// Admin function to migrate old schedules
function migrateToPercentageBased(bytes32 lotId) external {
    _validateCaller(lotId); // Only token can migrate
    
    VestingSchedule storage schedule = schedules[lotId];
    if (schedule.totalAmount == 0) revert("No schedule");
    if (schedule.totalBasisPoints > 0) revert("Already migrated");
    
    // Get original lot quantity
    uint256 originalQuantity = ITokenLots(lotToToken[lotId]).getLot(lotId).quantity;
    
    // Convert to percentage (assume totalAmount was 100% of original lot)
    schedule.totalBasisPoints = 10000; // 100%
    schedule.releasedBasisPoints = (schedule.releasedAmount * 10000) / originalQuantity;
    
    // Clear old fields (optional, saves gas on future reads)
    delete schedule.totalAmount;
    delete schedule.releasedAmount;
}
```

### Phase 3: Volume Limit Review

**Check current implementation**:
```solidity
// In VolumeLimitModule - need to verify this adapts to corporate actions
mapping(address => mapping(address => uint256)) public currentVolume;

// Question: When a stock split happens, does currentVolume reset?
// Or do we need to track volume as percentage too?
```

**Likely fix**: Track volume as basis points of total balance:
```solidity
struct VolumeLimitConfig {
    uint256 limitBps; // e.g., 100 = 1%
    uint256 windowSeconds; // e.g., 7776000 = 90 days
    uint256 currentVolumeBps; // Track as percentage, not absolute
    uint256 windowStart;
}
```

## Testing Requirements

### Security Tests:
```solidity
// Test: Attacker cannot set vesting on other token's lots
function testCannotSetVestingOnOtherTokensLots() public {
    // Token A creates lot
    vm.prank(tokenA);
    bytes32 lotId = keccak256(...);
    
    // Attacker tries to set vesting
    vm.prank(attacker);
    vm.expectRevert(Vesting_UnauthorizedCaller.selector);
    vestingModule.createVestingSchedule(lotId, attacker, 1e18, ...);
}

// Test: Only registered token can update schedule
function testOnlyRegisteredTokenCanUpdate() public {
    vm.prank(tokenA);
    vestingModule.createVestingSchedule(lotId, holder, 1e18, ...);
    
    // Token B tries to update
    vm.prank(tokenB);
    vm.expectRevert(Vesting_UnauthorizedCaller.selector);
    vestingModule.revokeVestingSchedule(lotId);
}
```

### Corporate Actions Tests:
```solidity
// Test: Vesting scales with 2:1 split
function testVestingScalesWithSplit() public {
    // Create 1M token lot with 4yr vesting
    createLotWithVesting(1_000_000e18, 4 years);
    
    // Fast forward 2 years (50% vested)
    skip(2 years);
    assertEq(vestingModule.getVestedAmount(lotId), 500_000e18);
    
    // Apply 2:1 split
    token.applyStockSplit(2, 1);
    
    // Now should have 1M vested (50% of 2M)
    assertEq(vestingModule.getVestedAmount(lotId), 1_000_000e18);
    
    // After 4 years total, all 2M should be vested
    skip(2 years);
    assertEq(vestingModule.getVestedAmount(lotId), 2_000_000e18);
}
```

## Summary

**Recommended Solution**:
1. ✅ Add `lotToToken` mapping to modules (caller validation)
2. ✅ Add wrapper functions to TokenComplianceFacet (admin auth)
3. ✅ Refactor to percentage-based vesting (corporate actions fix)
4. ✅ Review VolumeLimit module for similar issues
5. ✅ Comprehensive security + integration tests

This provides:
- **Security**: Defense in depth (caller validation + wrapper auth)
- **Simplicity**: One mapping, clear ownership model
- **Correctness**: Percentage-based vesting auto-scales
- **Maintainability**: Clean separation, easy to reason about




