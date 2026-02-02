# CapSign Pre-Launch Implementation Plan

**Created**: January 26, 2026  
**Total Estimated Effort**: 4-6 weeks  
**Goal**: 90-95% coverage of US private capital use cases

---

## Architecture Principles

**Source of Truth**: On-chain protocol (smart contracts)

```
┌─────────────────────────────────────────────────────────────────┐
│                    PROTOCOL (Source of Truth)                    │
│  VehicleCoreFacet, VehicleDistributionFacet, CapitalCallFacet   │
│  Events: CapitalCallCreated, NAVUpdated, TransferRequested      │
└─────────────────────────────┬───────────────────────────────────┘
                              │ Events indexed
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    SUBGRAPH (Indexer)                            │
│  Vehicle, CapitalCall, Distribution, NAVUpdate entities         │
└─────────────────────────────┬───────────────────────────────────┘
                              │ Goldsky Mirror pipeline
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    DATABASE (Read Replica)                       │
│  Fast queries, joins, filtering, off-chain metadata             │
└─────────────────────────────┬───────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    INTERFACE (UI)                                │
│  Reads: Subgraph/Mirror | Writes: Protocol via UserOperations   │
└─────────────────────────────────────────────────────────────────┘
```

---

## Current State Assessment

### ✅ Already On-Chain (Protocol)

| Feature | Facet/Interface | Events |
|---------|-----------------|--------|
| Vehicle Types | `IVehicle.VehicleType` | SPV, PE_FUND, VC_FUND, HEDGE_FUND, REAL_ESTATE_FUND |
| Capital Contributions | `VehicleCoreFacet` | `CapitalContributed` |
| Distributions | `VehicleDistributionFacet` | `DistributionCreated`, `DistributionClaimed`, `DistributionCancelled` |
| Waterfall Config | `IVehicleDistribution` | `WaterfallConfigured` |
| Investment Tracking | `VehicleInvestmentFacet` | `InvestmentMade`, `ValuationUpdated`, `InvestmentExited` |
| Member Management | `VehicleMembersFacet` | `MemberAdded`, `MemberRemoved` |
| Vehicle Lifecycle | `VehicleCoreFacet` | `VehicleInitialized`, `VehicleStatusChanged` |

### ✅ Already Indexed (Subgraph)

| Entity | Fields | Status |
|--------|--------|--------|
| `Vehicle` | totalCapitalContributed, totalDistributions, memberCount | ✅ Indexed |
| `VehicleMember` | capitalContributed, distributionsReceived, tokenBalance | ✅ Indexed |
| `CapitalContribution` | member, amount, timestamp | ✅ Indexed |
| `Distribution` | totalAmount, totalClaimed, lpAmount, carryAmount | ✅ Indexed |
| `DistributionClaim` | claimant, amount, claimed | ✅ Indexed |
| `VehicleInvestment` | target, amount, currentValuation, exited | ✅ Indexed |
| `VehicleValuation` | totalNAV, navPerToken, totalAssets, totalLiabilities | ✅ Indexed |

### ❌ Missing (Needs Implementation)

| Feature | Protocol | Subgraph | Interface | Priority |
|---------|----------|----------|-----------|----------|
| Capital Call Workflow | ❌ New facet | ❌ New entity | ❌ | P0 |
| ROFR Module | 🟡 Referenced | ❌ | ❌ | P0 |
| Fund-level NAV Event | 🟡 Investment-only | 🟡 Partial | ❌ | P0 |
| Transfer Approval | ❌ | ❌ | ❌ | P0 |
| K-1 Tax Allocation | ❌ | ❌ | ❌ | P0 |

---

## Recommended Implementation Order

Based on dependency analysis and on-chain-first architecture:

```
┌─────────────────────────────────────────────────────────────────┐
│             PHASE 1: PROTOCOL + SUBGRAPH (Week 1-2)             │
├─────────────────────────────────────────────────────────────────┤
│  1. CapitalCallFacet (protocol) ───────────────────────────────►│
│  2. VehicleNAVFacet (protocol) ────────────────────────────────►│
│  3. ROFRModule (compliance module) ────────────────────────────►│
│  4. Subgraph entities + mappings ──────────────────────────────►│
│  5. Goldsky Mirror configuration ──────────────────────────────►│
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│             PHASE 2: INTERFACE FOUNDATION (Week 2-3)            │
├─────────────────────────────────────────────────────────────────┤
│  6. Fund Admin Dashboard (reads from subgraph) ────────────────►│
│  7. Capital Call UI (writes to protocol) ──────────────────────►│
│  8. NAV Update UI ─────────────────────────────────────────────►│
│  9. Performance Metrics (calculated from subgraph) ────────────►│
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│             PHASE 3: ADVANCED FEATURES (Week 3-4)               │
├─────────────────────────────────────────────────────────────────┤
│  10. ROFR Transfer UI ─────────────────────────────────────────►│
│  11. Multi-Currency (stablecoin addresses) ────────────────────►│
│  12. Capital Account Statements (from subgraph data) ──────────►│
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│             PHASE 4: TAX + POLISH (Week 4-5)                    │
├─────────────────────────────────────────────────────────────────┤
│  13. K-1 Generation (from on-chain allocation data) ──────────►│
│  14. Custodian Integration Polish ─────────────────────────────►│
│  15. E2E Tests for all flows ──────────────────────────────────►│
└─────────────────────────────────────────────────────────────────┘
```

---

## Phase 1: Protocol + Subgraph

### 1.1 CapitalCallFacet (Protocol)

**Priority**: P0  
**Effort**: 2-3 days  
**Location**: `protocol/packages/wallets/src/facets/vehicle/VehicleCapitalCallFacet.sol`

#### Solidity Interface

```solidity
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.30;

interface IVehicleCapitalCall {
    // ============ Enums ============
    
    enum CapitalCallStatus {
        DRAFT,      // Created, not yet sent
        PENDING,    // Notices sent, awaiting funding
        PARTIAL,    // Some allocations funded
        FUNDED,     // All allocations funded
        DEFAULTED   // Past due with unfunded allocations
    }

    // ============ Structs ============
    
    struct CapitalCall {
        uint256 id;
        uint256 totalAmount;
        uint256 fundedAmount;
        uint256 dueDate;
        bytes32 purposeHash;    // IPFS hash of purpose/memo
        CapitalCallStatus status;
        address paymentToken;
        uint256 createdAt;
    }
    
    struct CallAllocation {
        address member;
        uint256 amount;
        uint256 fundedAmount;
        bool defaulted;
    }

    // ============ Events ============
    
    /// @notice Emitted when capital call is created
    event CapitalCallCreated(
        uint256 indexed callId,
        uint256 totalAmount,
        uint256 dueDate,
        address indexed paymentToken,
        bytes32 purposeHash
    );
    
    /// @notice Emitted when call notices are sent
    event CapitalCallNoticesSent(
        uint256 indexed callId,
        uint256 memberCount
    );
    
    /// @notice Emitted when member funds their allocation
    event CapitalCallFunded(
        uint256 indexed callId,
        address indexed member,
        uint256 amount
    );
    
    /// @notice Emitted when call is fully funded
    event CapitalCallCompleted(uint256 indexed callId);
    
    /// @notice Emitted when member defaults
    event CapitalCallDefault(
        uint256 indexed callId,
        address indexed member,
        uint256 unfundedAmount
    );

    // ============ Functions ============
    
    /// @notice Create a capital call (GP only)
    function createCapitalCall(
        uint256 amount,
        uint256 dueDate,
        address paymentToken,
        bytes32 purposeHash
    ) external returns (uint256 callId);
    
    /// @notice Send call notices (sets status to PENDING)
    function sendCapitalCallNotices(uint256 callId) external;
    
    /// @notice Fund allocation (member or GP on behalf)
    function fundCapitalCall(uint256 callId, uint256 amount) external;
    
    /// @notice Mark defaulted allocations (after due date)
    function processCapitalCallDefaults(uint256 callId) external;
    
    /// @notice Get call details
    function getCapitalCall(uint256 callId) external view returns (CapitalCall memory);
    
    /// @notice Get member allocation
    function getCallAllocation(uint256 callId, address member) external view returns (CallAllocation memory);
}
```

#### Unit Tests (Foundry)

```solidity
// test/vehicle/VehicleCapitalCallFacet.t.sol

function test_createCapitalCall() public {
    uint256 callId = vehicle.createCapitalCall(
        1_000_000e6,  // 1M USDC
        block.timestamp + 30 days,
        address(usdc),
        keccak256("Series A investment")
    );
    
    IVehicleCapitalCall.CapitalCall memory call = vehicle.getCapitalCall(callId);
    assertEq(call.totalAmount, 1_000_000e6);
    assertEq(uint8(call.status), uint8(IVehicleCapitalCall.CapitalCallStatus.DRAFT));
}

function test_fundCapitalCall() public {
    // Setup: create call, send notices
    vm.prank(lp1);
    vehicle.fundCapitalCall(callId, 100_000e6);
    
    IVehicleCapitalCall.CallAllocation memory alloc = vehicle.getCallAllocation(callId, lp1);
    assertEq(alloc.fundedAmount, 100_000e6);
}

function test_processDefaults() public {
    // Warp past due date
    vm.warp(block.timestamp + 31 days);
    vehicle.processCapitalCallDefaults(callId);
    
    IVehicleCapitalCall.CapitalCall memory call = vehicle.getCapitalCall(callId);
    assertEq(uint8(call.status), uint8(IVehicleCapitalCall.CapitalCallStatus.DEFAULTED));
}
```

---

### 1.2 VehicleNAVFacet (Protocol)

**Priority**: P0  
**Effort**: 1-2 days  
**Location**: `protocol/packages/wallets/src/facets/vehicle/VehicleNAVFacet.sol`

#### Solidity Interface

```solidity
interface IVehicleNAV {
    // ============ Enums ============
    
    enum NAVSource {
        ADMIN,          // Manual GP entry
        AUDITOR,        // Third-party audit
        ORACLE,         // Price feed
        QUARTERLY       // Quarterly valuation
    }

    // ============ Structs ============
    
    struct NAVUpdate {
        uint256 totalNAV;
        uint256 navPerToken;
        uint256 totalAssets;
        uint256 totalLiabilities;
        NAVSource source;
        bytes32 documentHash;  // IPFS hash of supporting docs
        address updatedBy;
        uint256 timestamp;
    }

    // ============ Events ============
    
    /// @notice Emitted when NAV is updated
    event NAVUpdated(
        uint256 indexed updateId,
        uint256 totalNAV,
        uint256 navPerToken,
        NAVSource source,
        bytes32 documentHash,
        address indexed updatedBy
    );

    // ============ Functions ============
    
    /// @notice Update NAV (admin only)
    function updateNAV(
        uint256 totalAssets,
        uint256 totalLiabilities,
        NAVSource source,
        bytes32 documentHash
    ) external returns (uint256 updateId);
    
    /// @notice Get current NAV
    function getCurrentNAV() external view returns (NAVUpdate memory);
    
    /// @notice Get NAV history count
    function getNAVHistoryCount() external view returns (uint256);
    
    /// @notice Get historical NAV
    function getNAVUpdate(uint256 updateId) external view returns (NAVUpdate memory);
}
```

---

### 1.3 ROFRModule (Protocol)

**Priority**: P0  
**Effort**: 2-3 days  
**Location**: `protocol/packages/tokens/src/compliance/modules/ROFRModule.sol`

#### Solidity Interface

```solidity
interface IROFRModule {
    // ============ Enums ============
    
    enum TransferRequestStatus {
        PENDING,        // Awaiting GP review
        ROFR_PERIOD,    // ROFR offer period active
        APPROVED,       // GP approved, ROFR expired/waived
        ROFR_EXERCISED, // Existing member matched offer
        REJECTED,       // GP rejected
        COMPLETED,      // Transfer executed
        CANCELLED       // Seller cancelled
    }

    // ============ Structs ============
    
    struct TransferRequest {
        uint256 id;
        address seller;
        address proposedBuyer;
        uint256 amount;
        uint256 pricePerToken;
        address paymentToken;
        TransferRequestStatus status;
        uint256 rofrExpiresAt;
        address rofrExercisedBy;
        uint256 createdAt;
    }

    // ============ Events ============
    
    event TransferRequested(
        uint256 indexed requestId,
        address indexed seller,
        address indexed proposedBuyer,
        uint256 amount,
        uint256 pricePerToken
    );
    
    event ROFRPeriodStarted(
        uint256 indexed requestId,
        uint256 expiresAt
    );
    
    event ROFRExercised(
        uint256 indexed requestId,
        address indexed exercisedBy
    );
    
    event TransferApproved(uint256 indexed requestId);
    event TransferRejected(uint256 indexed requestId, string reason);
    event TransferCompleted(uint256 indexed requestId);

    // ============ Functions ============
    
    /// @notice Request transfer (seller)
    function requestTransfer(
        address buyer,
        uint256 amount,
        uint256 pricePerToken,
        address paymentToken
    ) external returns (uint256 requestId);
    
    /// @notice Start ROFR period (GP)
    function startROFRPeriod(uint256 requestId, uint256 duration) external;
    
    /// @notice Exercise ROFR (existing member)
    function exerciseROFR(uint256 requestId) external;
    
    /// @notice Approve transfer (GP, after ROFR expires)
    function approveTransfer(uint256 requestId) external;
    
    /// @notice Reject transfer (GP)
    function rejectTransfer(uint256 requestId, string calldata reason) external;
    
    /// @notice Execute approved transfer
    function executeTransfer(uint256 requestId) external;
}
```

---

### 1.4 Subgraph Schema Updates

**Location**: `subgraph/schema.graphql`

```graphql
# ============ CAPITAL CALL ENTITIES ============

type CapitalCall @entity {
  id: ID! # vehicle-callId
  vehicle: Vehicle!
  callId: BigInt!
  totalAmount: BigInt!
  fundedAmount: BigInt!
  dueDate: BigInt!
  purposeHash: Bytes!
  status: CapitalCallStatus!
  paymentToken: Bytes!
  createdAt: BigInt!
  createdTx: Bytes!
  
  # Allocations
  allocations: [CapitalCallAllocation!]! @derivedFrom(field: "capitalCall")
  
  # Completion
  completedAt: BigInt
  completedTx: Bytes
}

enum CapitalCallStatus {
  DRAFT
  PENDING
  PARTIAL
  FUNDED
  DEFAULTED
}

type CapitalCallAllocation @entity {
  id: ID! # capitalCall-member
  capitalCall: CapitalCall!
  member: VehicleMember!
  memberAddress: Bytes!
  amount: BigInt!
  fundedAmount: BigInt!
  defaulted: Boolean!
  fundedAt: BigInt
  fundedTx: Bytes
}

# ============ NAV ENTITIES ============

type NAVUpdate @entity {
  id: ID! # vehicle-updateId or tx-logIndex
  vehicle: Vehicle!
  updateId: BigInt!
  totalNAV: BigInt!
  navPerToken: BigInt!
  totalAssets: BigInt!
  totalLiabilities: BigInt!
  source: NAVSource!
  documentHash: Bytes
  updatedBy: Bytes!
  timestamp: BigInt!
  tx: Bytes!
  blockNumber: BigInt!
}

enum NAVSource {
  ADMIN
  AUDITOR
  ORACLE
  QUARTERLY
}

# ============ ROFR ENTITIES ============

type TransferRequest @entity {
  id: ID! # token-requestId
  token: Token!
  requestId: BigInt!
  seller: Bytes!
  proposedBuyer: Bytes!
  amount: BigInt!
  pricePerToken: BigInt!
  paymentToken: Bytes!
  status: TransferRequestStatus!
  rofrExpiresAt: BigInt
  rofrExercisedBy: Bytes
  createdAt: BigInt!
  createdTx: Bytes!
  
  # Resolution
  resolvedAt: BigInt
  resolvedTx: Bytes
  rejectionReason: String
}

enum TransferRequestStatus {
  PENDING
  ROFR_PERIOD
  APPROVED
  ROFR_EXERCISED
  REJECTED
  COMPLETED
  CANCELLED
}
```

---

### 1.5 Subgraph Mappings

**Location**: `subgraph/src/mappings/vehicle.ts`

```typescript
import { CapitalCallCreated, CapitalCallFunded, CapitalCallCompleted } from '../generated/templates/VehicleDiamond/VehicleCapitalCallFacet';
import { NAVUpdated } from '../generated/templates/VehicleDiamond/VehicleNAVFacet';
import { CapitalCall, CapitalCallAllocation, NAVUpdate } from '../generated/schema';

export function handleCapitalCallCreated(event: CapitalCallCreated): void {
  const vehicleId = event.address.toHexString();
  const callId = event.params.callId;
  const id = vehicleId + '-' + callId.toString();
  
  let call = new CapitalCall(id);
  call.vehicle = vehicleId;
  call.callId = callId;
  call.totalAmount = event.params.totalAmount;
  call.fundedAmount = BigInt.zero();
  call.dueDate = event.params.dueDate;
  call.purposeHash = event.params.purposeHash;
  call.status = 'DRAFT';
  call.paymentToken = event.params.paymentToken;
  call.createdAt = event.block.timestamp;
  call.createdTx = event.transaction.hash;
  call.save();
}

export function handleCapitalCallFunded(event: CapitalCallFunded): void {
  const vehicleId = event.address.toHexString();
  const callId = vehicleId + '-' + event.params.callId.toString();
  const allocId = callId + '-' + event.params.member.toHexString();
  
  let alloc = CapitalCallAllocation.load(allocId);
  if (!alloc) {
    alloc = new CapitalCallAllocation(allocId);
    alloc.capitalCall = callId;
    alloc.memberAddress = event.params.member;
    alloc.amount = BigInt.zero(); // Will be set by allocation event
    alloc.fundedAmount = BigInt.zero();
    alloc.defaulted = false;
  }
  
  alloc.fundedAmount = alloc.fundedAmount.plus(event.params.amount);
  alloc.fundedAt = event.block.timestamp;
  alloc.fundedTx = event.transaction.hash;
  alloc.save();
  
  // Update call funded amount
  let call = CapitalCall.load(callId);
  if (call) {
    call.fundedAmount = call.fundedAmount.plus(event.params.amount);
    if (call.fundedAmount >= call.totalAmount) {
      call.status = 'FUNDED';
    } else {
      call.status = 'PARTIAL';
    }
    call.save();
  }
}

export function handleNAVUpdated(event: NAVUpdated): void {
  const vehicleId = event.address.toHexString();
  const id = event.transaction.hash.toHexString() + '-' + event.logIndex.toString();
  
  let nav = new NAVUpdate(id);
  nav.vehicle = vehicleId;
  nav.updateId = event.params.updateId;
  nav.totalNAV = event.params.totalNAV;
  nav.navPerToken = event.params.navPerToken;
  nav.source = mapNAVSource(event.params.source);
  nav.documentHash = event.params.documentHash;
  nav.updatedBy = event.params.updatedBy;
  nav.timestamp = event.block.timestamp;
  nav.tx = event.transaction.hash;
  nav.blockNumber = event.block.number;
  nav.save();
  
  // Update Vehicle current NAV
  let vehicle = Vehicle.load(vehicleId);
  if (vehicle) {
    vehicle.currentNAV = event.params.totalNAV;
    vehicle.navPerToken = event.params.navPerToken;
    vehicle.lastNAVUpdate = event.block.timestamp;
    vehicle.save();
  }
}
```

---

### 1.6 Goldsky Mirror Configuration

**Location**: `subgraph/goldsky-mirror.yaml`

```yaml
version: "1.0"
name: capsign-mirror
source:
  type: subgraph
  subgraph_id: ${SUBGRAPH_DEPLOYMENT_ID}
  
destinations:
  - type: postgres
    connection_string: ${DATABASE_URL}
    tables:
      # Existing tables...
      
      - name: capital_calls
        entity: CapitalCall
        columns:
          - id
          - vehicle_id: vehicle
          - call_id: callId
          - total_amount: totalAmount
          - funded_amount: fundedAmount
          - due_date: dueDate
          - purpose_hash: purposeHash
          - status
          - payment_token: paymentToken
          - created_at: createdAt
          - created_tx: createdTx
          - completed_at: completedAt
          - completed_tx: completedTx
          
      - name: capital_call_allocations
        entity: CapitalCallAllocation
        columns:
          - id
          - capital_call_id: capitalCall
          - member_id: member
          - member_address: memberAddress
          - amount
          - funded_amount: fundedAmount
          - defaulted
          - funded_at: fundedAt
          - funded_tx: fundedTx
          
      - name: nav_updates
        entity: NAVUpdate
        columns:
          - id
          - vehicle_id: vehicle
          - update_id: updateId
          - total_nav: totalNAV
          - nav_per_token: navPerToken
          - total_assets: totalAssets
          - total_liabilities: totalLiabilities
          - source
          - document_hash: documentHash
          - updated_by: updatedBy
          - timestamp
          - tx
          - block_number: blockNumber
          
      - name: transfer_requests
        entity: TransferRequest
        columns:
          - id
          - token_id: token
          - request_id: requestId
          - seller
          - proposed_buyer: proposedBuyer
          - amount
          - price_per_token: pricePerToken
          - payment_token: paymentToken
          - status
          - rofr_expires_at: rofrExpiresAt
          - rofr_exercised_by: rofrExercisedBy
          - created_at: createdAt
```

---

## Phase 2: Interface Foundation

### 2.1 Fund Admin Dashboard (Interface)

**Priority**: P0  
**Effort**: 3-4 days  
**Dependencies**: Subgraph indexing capital calls, NAV, distributions

#### Data Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                        GraphQL Query                             │
│  query GetVehicle($id: ID!) {                                   │
│    vehicle(id: $id) {                                           │
│      id                                                         │
│      vehicleType                                                │
│      totalCapitalContributed                                    │
│      totalDistributionsExecuted                                 │
│      memberCount                                                │
│      currentNAV                                                 │
│      navPerToken                                                │
│      capitalCalls { ... }                                       │
│      distributions { ... }                                      │
│      members { capitalContributed, distributionsReceived }      │
│    }                                                            │
│  }                                                              │
└─────────────────────────────────────────────────────────────────┘
```

#### UI Pages

```
/vehicles                              # All vehicles (GP view)
/vehicles/[address]                    # Vehicle dashboard
/vehicles/[address]/capital-calls      # Capital call management
/vehicles/[address]/distributions      # Distribution management
/vehicles/[address]/nav                # NAV history
/vehicles/[address]/members            # Member management
/vehicles/[address]/settings           # Waterfall, fees

/investor/vehicles                     # My investments (LP view)
/investor/vehicles/[address]           # Investment detail
```

#### E2E Tests

```typescript
// e2e/tests/flows/fund-dashboard.spec.ts
test.describe('Fund Admin Dashboard', () => {
  test('@smoke GP can view vehicle overview from subgraph', async ({ page }) => {
    await page.goto('/vehicles/0x1234...'); // Vehicle address
    
    // All data comes from subgraph
    await expect(page.getByText('Total Capital')).toBeVisible();
    await expect(page.getByText('Total Distributions')).toBeVisible();
    await expect(page.getByText('Current NAV')).toBeVisible();
    await expect(page.getByText('Members')).toBeVisible();
  });

  test('capital calls list loads from subgraph', async ({ page }) => {
    await page.goto('/vehicles/0x1234.../capital-calls');
    await expect(page.getByRole('table')).toBeVisible();
    // Verify on-chain data is displayed
  });

  test('LP can view their investment', async ({ page }) => {
    await page.goto('/investor/vehicles/0x1234...');
    await expect(page.getByText('My Contribution')).toBeVisible();
    await expect(page.getByText('Distributions Received')).toBeVisible();
    await expect(page.getByText('Ownership %')).toBeVisible();
  });
});
```

---

### 2.2 Capital Call UI (Interface)

**Priority**: P0  
**Effort**: 2-3 days  
**Dependencies**: CapitalCallFacet deployed, subgraph indexing

#### User Operations (Writes to Protocol)

```typescript
// lib/vehicle/capital-calls.ts
import { encodeFunctionData } from 'viem';
import { executeUserOperation } from '@/lib/aa/operations';

export async function createCapitalCall(
  vehicleAddress: Address,
  amount: bigint,
  dueDate: number,
  paymentToken: Address,
  purposeHash: `0x${string}`
) {
  const callData = encodeFunctionData({
    abi: VehicleCapitalCallFacetABI,
    functionName: 'createCapitalCall',
    args: [amount, BigInt(dueDate), paymentToken, purposeHash],
  });
  
  return executeUserOperation({
    to: vehicleAddress,
    data: callData,
  });
}

export async function fundCapitalCall(
  vehicleAddress: Address,
  callId: bigint,
  amount: bigint
) {
  const callData = encodeFunctionData({
    abi: VehicleCapitalCallFacetABI,
    functionName: 'fundCapitalCall',
    args: [callId, amount],
  });
  
  return executeUserOperation({
    to: vehicleAddress,
    data: callData,
  });
}
```

#### E2E Tests

```typescript
// e2e/tests/flows/capital-call.spec.ts
test.describe('Capital Call Workflow', () => {
  test('GP can create capital call (on-chain)', async ({ page }) => {
    await page.goto('/vehicles/0x1234.../capital-calls');
    await page.click('text=New Capital Call');
    
    await page.fill('[name="amount"]', '1000000');
    await page.fill('[name="dueDate"]', '2026-02-15');
    await page.fill('[name="purpose"]', 'Series A investment');
    await page.click('text=Create Call');
    
    // Transaction modal appears
    await expect(page.getByText('Confirm Transaction')).toBeVisible();
    await page.click('text=Confirm');
    
    // Wait for on-chain confirmation
    await expect(page.getByText('Capital call created')).toBeVisible();
    
    // Verify in subgraph (may need to wait for indexing)
    await page.reload();
    await expect(page.getByText('Series A investment')).toBeVisible();
  });

  test('LP can fund capital call (on-chain)', async ({ page }) => {
    await page.goto('/investor/vehicles/0x1234.../capital-calls/1');
    await page.click('text=Fund Capital Call');
    
    // Transaction modal
    await expect(page.getByText('Confirm Transaction')).toBeVisible();
    await page.click('text=Confirm');
    
    await expect(page.getByText('Capital call funded')).toBeVisible();
  });
});
```

---

### 2.3 NAV Update UI (Interface)

**Priority**: P0  
**Effort**: 1-2 days  
**Dependencies**: VehicleNAVFacet deployed, subgraph indexing

#### User Operations

```typescript
// lib/vehicle/nav.ts
export async function updateNAV(
  vehicleAddress: Address,
  totalAssets: bigint,
  totalLiabilities: bigint,
  source: number, // NAVSource enum
  documentHash: `0x${string}`
) {
  const callData = encodeFunctionData({
    abi: VehicleNAVFacetABI,
    functionName: 'updateNAV',
    args: [totalAssets, totalLiabilities, source, documentHash],
  });
  
  return executeUserOperation({
    to: vehicleAddress,
    data: callData,
  });
}
```

#### E2E Tests

```typescript
// e2e/tests/flows/nav-workflow.spec.ts
test.describe('NAV Update Workflow', () => {
  test('GP can record NAV update (on-chain)', async ({ page }) => {
    await page.goto('/vehicles/0x1234.../nav');
    await page.click('text=Update NAV');
    
    await page.fill('[name="totalAssets"]', '15000000');
    await page.fill('[name="totalLiabilities"]', '500000');
    await page.selectOption('[name="source"]', 'QUARTERLY');
    // Optional: attach document
    await page.click('text=Submit NAV Update');
    
    // Transaction modal
    await expect(page.getByText('Confirm Transaction')).toBeVisible();
    await page.click('text=Confirm');
    
    // Wait for on-chain confirmation
    await expect(page.getByText('NAV updated')).toBeVisible();
  });

  test('NAV history shows on-chain audit trail', async ({ page }) => {
    await page.goto('/vehicles/0x1234.../nav');
    await expect(page.getByRole('table')).toBeVisible();
    // Each row links to block explorer tx
    await expect(page.getByText('View on Explorer')).toBeVisible();
  });

  test('LP can view NAV but not update', async ({ page }) => {
    await page.goto('/investor/vehicles/0x1234...');
    await expect(page.getByText('Current NAV')).toBeVisible();
    await expect(page.getByText('Update NAV')).not.toBeVisible();
  });
});
```

---

### 2.4 Performance Metrics (Interface)

**Priority**: P0  
**Effort**: 2-3 days  
**Dependencies**: NAV + Capital data indexed in subgraph

#### Data Source: Subgraph

Performance metrics are **calculated client-side** from on-chain data:

```typescript
// lib/vehicle/metrics.ts

/**
 * Calculate metrics from subgraph data
 * All inputs come from on-chain events
 */
export function calculateVehicleMetrics(vehicleData: VehicleSubgraphData) {
  const { 
    contributions,      // CapitalContribution entities
    distributions,      // DistributionClaim entities
    navUpdates,         // NAVUpdate entities
    members,            // VehicleMember entities
  } = vehicleData;
  
  const totalContributed = sumBigInt(contributions.map(c => c.amount));
  const totalDistributed = sumBigInt(distributions.map(d => d.amount));
  const currentNAV = navUpdates[0]?.totalNAV || 0n;
  
  return {
    irr: calculateIRR(contributions, distributions, currentNAV),
    moic: calculateMOIC(totalDistributed, currentNAV, totalContributed),
    tvpi: calculateTVPI(totalDistributed, currentNAV, totalContributed),
    dpi: calculateDPI(totalDistributed, totalContributed),
    rvpi: calculateRVPI(currentNAV, totalContributed),
  };
}

/**
 * Calculate member-specific metrics
 */
export function calculateMemberMetrics(
  memberAddress: Address,
  vehicleData: VehicleSubgraphData
) {
  const memberContributions = vehicleData.contributions.filter(
    c => c.member === memberAddress
  );
  const memberDistributions = vehicleData.distributionClaims.filter(
    c => c.claimantAddress === memberAddress
  );
  const member = vehicleData.members.find(m => m.memberAddress === memberAddress);
  
  // Calculate NAV allocation based on token balance
  const memberNAVShare = (member?.tokenBalance / totalTokenSupply) * currentNAV;
  
  return {
    irr: calculateIRR(memberContributions, memberDistributions, memberNAVShare),
    // ... other metrics
  };
}
```

#### E2E Tests

```typescript
// e2e/tests/flows/performance-metrics.spec.ts
test.describe('Performance Metrics', () => {
  test('vehicle dashboard shows metrics from on-chain data', async ({ page }) => {
    await page.goto('/vehicles/0x1234...');
    
    // Metrics calculated from subgraph data
    await expect(page.getByText('IRR')).toBeVisible();
    await expect(page.getByText('MOIC')).toBeVisible();
    await expect(page.getByText('TVPI')).toBeVisible();
    await expect(page.getByText('DPI')).toBeVisible();
  });

  test('metrics recalculate when NAV updates', async ({ page }) => {
    // Record initial metrics
    await page.goto('/vehicles/0x1234...');
    const initialTVPI = await page.getByTestId('tvpi-value').textContent();
    
    // Update NAV (on-chain)
    await page.click('text=Update NAV');
    // ... complete update flow
    
    // Wait for subgraph to index
    await page.waitForTimeout(5000);
    await page.reload();
    
    // Verify metrics changed
    const newTVPI = await page.getByTestId('tvpi-value').textContent();
    expect(newTVPI).not.toBe(initialTVPI);
  });

  test('LP sees their specific metrics', async ({ page }) => {
    await page.goto('/investor/vehicles/0x1234...');
    await expect(page.getByText('Your IRR')).toBeVisible();
    await expect(page.getByText('Your TVPI')).toBeVisible();
  });
});
```

---

## Phase 3: Advanced Features

### 3.1 ROFR Transfer UI (Interface)

**Priority**: P0  
**Effort**: 2-3 days  
**Dependencies**: ROFRModule deployed, subgraph indexing

#### On-Chain Workflow

```
┌─────────────────────────────────────────────────────────────────┐
│  1. Seller calls requestTransfer() ──────────────────────────► │
│     Event: TransferRequested                                    │
│     Status: PENDING                                             │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  2. GP calls startROFRPeriod() ──────────────────────────────► │
│     Event: ROFRPeriodStarted                                    │
│     Status: ROFR_PERIOD                                         │
│     Sets: rofrExpiresAt                                         │
└─────────────────────────────────────────────────────────────────┘
                              │
              ┌───────────────┴───────────────┐
              ▼                               ▼
┌─────────────────────────────┐   ┌─────────────────────────────┐
│  3a. Existing LP calls      │   │  3b. ROFR expires, GP calls │
│      exerciseROFR()         │   │      approveTransfer()      │
│  Event: ROFRExercised       │   │  Event: TransferApproved    │
│  Status: ROFR_EXERCISED     │   │  Status: APPROVED           │
└─────────────────────────────┘   └─────────────────────────────┘
              │                               │
              └───────────────┬───────────────┘
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  4. Buyer/ROFR winner calls executeTransfer() ───────────────► │
│     Event: TransferCompleted                                    │
│     Status: COMPLETED                                           │
│     Tokens transferred on-chain                                 │
└─────────────────────────────────────────────────────────────────┘
```

#### E2E Tests

```typescript
// e2e/tests/flows/rofr-workflow.spec.ts
test.describe('ROFR Workflow (On-Chain)', () => {
  test('LP can request transfer (on-chain)', async ({ page }) => {
    await page.goto('/investor/vehicles/0x1234...');
    await page.click('text=Request Transfer');
    
    await page.fill('[name="amount"]', '100');
    await page.fill('[name="pricePerToken"]', '15.00');
    await page.fill('[name="buyerAddress"]', '0xBuyer...');
    await page.click('text=Submit Request');
    
    // On-chain transaction
    await expect(page.getByText('Confirm Transaction')).toBeVisible();
    await page.click('text=Confirm');
    
    await expect(page.getByText('Transfer request submitted')).toBeVisible();
  });

  test('GP can start ROFR period', async ({ page }) => {
    await page.goto('/vehicles/0x1234.../transfers/1');
    await page.click('text=Start ROFR Period');
    await page.fill('[name="duration"]', '14'); // 14 days
    await page.click('text=Confirm');
    
    await expect(page.getByText('ROFR period started')).toBeVisible();
  });

  test('existing LP can exercise ROFR (on-chain)', async ({ page }) => {
    await page.goto('/investor/rofr/0x1234...-1');
    
    // Shows offer details
    await expect(page.getByText('100 tokens @ $15.00')).toBeVisible();
    await expect(page.getByText('ROFR expires in')).toBeVisible();
    
    await page.click('text=Exercise ROFR');
    await page.click('text=Confirm');
    
    await expect(page.getByText('ROFR exercised')).toBeVisible();
  });
});
```

---

### 3.2 Multi-Currency Support (Interface)

**Priority**: P0  
**Effort**: 2-3 days  
**Dependencies**: Payment token addresses on mainnet

#### Implementation

```typescript
// constants/stablecoins.ts
export const STABLECOINS = {
  84532: { // Base Sepolia
    USDC: '0x036CbD53842c5426634e7929541eC2318f3dCF7e',
    // EURC not available on testnet
  },
  8453: { // Base Mainnet
    USDC: '0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913',
    EURC: '0x60a3E35Cc302bFA44Cb288Bc5a4F316Fdb1adb42', // Circle EUR
  },
} as const;

export const CURRENCY_INFO: Record<string, CurrencyInfo> = {
  USDC: { symbol: 'USDC', decimals: 6, name: 'USD Coin', currencySymbol: '$' },
  EURC: { symbol: 'EURC', decimals: 6, name: 'Euro Coin', currencySymbol: '€' },
};

// Helper to get currency by address
export function getCurrencyByAddress(address: Address, chainId: number): CurrencyInfo | null {
  const chains = STABLECOINS[chainId as keyof typeof STABLECOINS];
  for (const [key, addr] of Object.entries(chains)) {
    if (addr.toLowerCase() === address.toLowerCase()) {
      return CURRENCY_INFO[key];
    }
  }
  return null;
}
```

#### E2E Tests

```typescript
// e2e/tests/flows/multi-currency.spec.ts
test.describe('Multi-Currency Support', () => {
  test('can create offering with EURC', async ({ page }) => {
    await page.goto('/offerings/create');
    
    await page.click('[data-testid="currency-selector"]');
    await page.click('text=EURC - Euro Coin');
    
    // Verify EURC is selected
    await expect(page.getByText('€')).toBeVisible();
  });

  test('displays correct currency symbol from on-chain data', async ({ page }) => {
    // Offering with EURC payment token
    await page.goto('/offerings/0xEURCOffering...');
    await expect(page.getByText('€500,000')).toBeVisible();
  });

  test('distribution amounts show correct currency', async ({ page }) => {
    await page.goto('/vehicles/0x1234.../distributions');
    // Distribution in EURC
    await expect(page.getByText('€100,000 EURC')).toBeVisible();
  });
});
```

---

### 3.3 Capital Account Statements (Interface)

**Priority**: P0  
**Effort**: 3 days  
**Dependencies**: All capital data indexed in subgraph

#### Data Source: Subgraph

Statements are **generated from on-chain data**:

```typescript
// lib/statements/capital-account.ts

interface CapitalAccountStatement {
  investor: {
    address: Address;
    name?: string; // Off-chain, from db
  };
  vehicle: {
    address: Address;
    name: string;
    vehicleType: VehicleType;
  };
  period: {
    start: Date;
    end: Date;
  };
  // All below from subgraph (on-chain source)
  openingBalance: {
    contributions: bigint;
    distributions: bigint;
    navValue: bigint;
  };
  activity: {
    capitalCalls: CapitalCallAllocation[]; // From subgraph
    distributions: DistributionClaim[];    // From subgraph
  };
  closingBalance: {
    contributions: bigint;
    distributions: bigint;
    navValue: bigint;
    ownershipPercent: number; // Calculated from token balance
  };
  metrics: VehicleMetrics; // Calculated from subgraph data
}

export async function generateStatement(
  vehicleAddress: Address,
  memberAddress: Address,
  periodEnd: Date
): Promise<CapitalAccountStatement> {
  // Query subgraph for all on-chain data
  const vehicleData = await queryVehicleData(vehicleAddress, memberAddress);
  
  // Filter to period
  const periodContributions = vehicleData.contributions.filter(
    c => Number(c.timestamp) <= periodEnd.getTime() / 1000
  );
  
  // ... build statement from on-chain data
}
```

#### PDF Generation

```typescript
// lib/pdf/capital-statement.tsx
import { Document, Page, Text, View, pdf } from '@react-pdf/renderer';

export async function generateStatementPDF(
  statement: CapitalAccountStatement
): Promise<Buffer> {
  const doc = (
    <Document>
      <Page size="LETTER">
        <View style={styles.header}>
          <Text>Capital Account Statement</Text>
          <Text>{statement.vehicle.name}</Text>
        </View>
        
        <View style={styles.section}>
          <Text>Opening Balance</Text>
          <Text>Contributions: {formatCurrency(statement.openingBalance.contributions)}</Text>
          <Text>Distributions: {formatCurrency(statement.openingBalance.distributions)}</Text>
        </View>
        
        <View style={styles.section}>
          <Text>Activity</Text>
          {statement.activity.capitalCalls.map(call => (
            <Text key={call.id}>
              Capital Call #{call.callId}: {formatCurrency(call.amount)}
            </Text>
          ))}
        </View>
        
        {/* ... closing balance, metrics */}
      </Page>
    </Document>
  );
  
  return pdf(doc).toBuffer();
}
```

#### E2E Tests

```typescript
// e2e/tests/flows/capital-statements.spec.ts
test.describe('Capital Account Statements', () => {
  test('LP can view statement from on-chain data', async ({ page }) => {
    await page.goto('/investor/vehicles/0x1234.../statement');
    
    await expect(page.getByText('Capital Account Statement')).toBeVisible();
    await expect(page.getByText('Opening Balance')).toBeVisible();
    await expect(page.getByText('Closing Balance')).toBeVisible();
    
    // Activity shows on-chain events
    await expect(page.getByText('Capital Call #1')).toBeVisible();
    await expect(page.getByText('Distribution #1')).toBeVisible();
  });

  test('LP can download PDF statement', async ({ page }) => {
    const downloadPromise = page.waitForEvent('download');
    await page.goto('/investor/vehicles/0x1234.../statement');
    await page.click('text=Download PDF');
    
    const download = await downloadPromise;
    expect(download.suggestedFilename()).toContain('capital-statement');
    expect(download.suggestedFilename()).toContain('.pdf');
  });

  test('GP can generate all statements for vehicle', async ({ page }) => {
    await page.goto('/vehicles/0x1234.../statements');
    await page.click('text=Generate All Statements');
    
    await expect(page.getByText('Generating statements for')).toBeVisible();
    // Should show count from on-chain member data
  });
});
```

---

## Phase 4: Tax + Polish

### 4.1 K-1 Generation (Interface)

**Priority**: P0  
**Effort**: 5+ days  
**Dependencies**: All capital/distribution data indexed

#### Architecture Decision

K-1 generation requires both:
1. **On-chain data**: Contributions, distributions, ownership %
2. **Off-chain data**: Partner TIN/SSN, addresses, tax allocations

**Recommended approach**: Store tax allocation percentages on-chain, sensitive PII in encrypted database.

#### K-1 Data Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                    ON-CHAIN (Subgraph)                          │
│  - Partner capital account (contributions, distributions)      │
│  - Ownership percentage (from token balance)                   │
│  - Distribution amounts and dates                              │
└─────────────────────────────────────────────────────────────────┘
                              +
┌─────────────────────────────────────────────────────────────────┐
│                    OFF-CHAIN (Encrypted DB)                     │
│  - Partner TIN/SSN (encrypted)                                 │
│  - Partner address                                             │
│  - Tax election choices                                        │
│  - Income allocation percentages (if different from ownership) │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                    K-1 GENERATION                               │
│  - Combine on-chain + off-chain data                           │
│  - Calculate tax items (ordinary income, cap gains, etc.)      │
│  - Generate IRS-compliant PDF                                  │
└─────────────────────────────────────────────────────────────────┘
```

#### E2E Tests

```typescript
// e2e/tests/flows/k1-generation.spec.ts
test.describe('K-1 Generation', () => {
  test('GP can generate K-1s from on-chain data', async ({ page }) => {
    await page.goto('/vehicles/0x1234.../tax');
    await page.selectOption('[name="taxYear"]', '2025');
    await page.click('text=Generate K-1s');
    
    await expect(page.getByText('K-1s generated')).toBeVisible();
    // Shows count from on-chain member list
  });

  test('K-1 capital account matches on-chain data', async ({ page }) => {
    await page.goto('/investor/tax/2025');
    await page.click('text=View K-1');
    
    // Capital account section
    await expect(page.getByText('Beginning Capital')).toBeVisible();
    await expect(page.getByText('Contributions')).toBeVisible();
    await expect(page.getByText('Distributions')).toBeVisible();
    await expect(page.getByText('Ending Capital')).toBeVisible();
    
    // Values should match subgraph data
  });

  test('LP can download K-1 PDF', async ({ page }) => {
    const downloadPromise = page.waitForEvent('download');
    await page.goto('/investor/tax/2025');
    await page.click('text=Download K-1');
    
    const download = await downloadPromise;
    expect(download.suggestedFilename()).toContain('k1-2025');
  });
});
```

---

### 4.2 Custodian Integration Polish (Interface)

**Priority**: P1  
**Effort**: 2-3 days  
**Dependencies**: Existing custodian API integrations

#### Current State

Basic custodian integration exists. Polish needed:
- Better error handling
- Transaction status sync
- Balance reconciliation

#### E2E Tests

```typescript
// e2e/tests/flows/custodian-integration.spec.ts
test.describe('Custodian Integration', () => {
  test('custodian balance syncs with on-chain balance', async ({ page }) => {
    await page.goto('/wallets/0x1234...');
    
    // Both should match
    await expect(page.getByText('On-Chain Balance')).toBeVisible();
    await expect(page.getByText('Custodian Balance')).toBeVisible();
    // Verify values are equal or show discrepancy alert
  });

  test('custodian transaction shows on-chain confirmation', async ({ page }) => {
    await page.goto('/wallets/0x1234.../transactions');
    
    // Each custodian tx should link to block explorer
    await expect(page.getByText('View on Explorer')).toBeVisible();
  });
});
```

---

## Testing Strategy

### Unit Tests (Foundry - Protocol)
```solidity
test/vehicle/
├── VehicleCapitalCallFacet.t.sol
├── VehicleNAVFacet.t.sol
├── ROFRModule.t.sol
└── VehicleDistributionFacet.t.sol

// Test coverage:
// - Capital call creation, funding, defaults
// - NAV updates, validation
// - ROFR workflow states
// - Distribution calculations
```

### Subgraph Tests
```typescript
// subgraph/tests/
// Test event handlers produce correct entities
// Verify derived fields calculate correctly
```

### Integration Tests (Interface)
```typescript
// Verify subgraph queries return expected data
// Test metric calculations from mock subgraph data
// PDF generation with sample data
```

### E2E Tests (Playwright)

**All E2E tests verify on-chain state:**

```
e2e/tests/flows/
├── fund-dashboard.spec.ts          # Reads from subgraph
├── nav-workflow.spec.ts            # Writes to protocol, reads from subgraph
├── capital-call.spec.ts            # Full on-chain workflow
├── distribution.spec.ts            # Claim-based on-chain flow
├── rofr-workflow.spec.ts           # Multi-party on-chain flow
├── capital-statements.spec.ts      # Generated from subgraph data
├── k1-generation.spec.ts           # On-chain + off-chain hybrid
├── performance-metrics.spec.ts     # Calculated from subgraph
├── multi-currency.spec.ts          # Payment token verification
└── custodian-integration.spec.ts   # Balance reconciliation
```

### On-Chain Test Data Seeding

```solidity
// script/seed/SeedTestVehicle.s.sol

// Deploy vehicle with:
// - 5 LPs with varying commitments (on-chain members)
// - 3 historical capital calls (on-chain events)
// - 2 distributions (on-chain claims)
// - 4 NAV updates (on-chain history)
// - 1 pending transfer request (ROFR flow)
```

---

## Recommendation: Start with Protocol (Phase 1.1-1.3)

**Why Protocol First?**
1. **Source of truth** - Everything else depends on on-chain data
2. **Testable in isolation** - Foundry tests before UI
3. **Unblocks subgraph** - Can't index events that don't exist
4. **Clear interface contracts** - Defines exactly what data UI will have

**Suggested Week 1 Approach:**

| Day | Task | Deliverable |
|-----|------|-------------|
| 1 | CapitalCallFacet interface + storage | `IVehicleCapitalCall.sol` |
| 2 | CapitalCallFacet implementation | `VehicleCapitalCallFacet.sol` |
| 3 | CapitalCallFacet unit tests | 10+ Foundry tests passing |
| 4 | VehicleNAVFacet (simpler) | Interface + implementation + tests |
| 5 | ROFRModule implementation | Compliance module + tests |

**Week 2: Subgraph + Interface**

| Day | Task | Deliverable |
|-----|------|-------------|
| 1-2 | Subgraph schema + mappings | New entities indexed |
| 3 | Deploy subgraph, configure Goldsky | Data flowing to mirror |
| 4-5 | Vehicle Dashboard UI | Reads from subgraph |

---

## Summary

This plan ensures:

1. **On-chain first** - Protocol is source of truth
2. **Proper data flow** - Protocol → Subgraph → Mirror → UI
3. **Audit trail** - All fund operations recorded on-chain
4. **E2E verification** - Tests confirm on-chain state matches UI

**Total effort remains 4-6 weeks**, but with correct architecture:
- Week 1-2: Protocol + Subgraph
- Week 2-3: Interface foundation
- Week 3-4: Advanced features
- Week 4-5: Tax reporting + polish
- Week 5-6: E2E testing

Would you like to start implementing the CapitalCallFacet?
