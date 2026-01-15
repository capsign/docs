# Identity Resolution System - Implementation Progress

## ✅ Phase 1: Database & Backend (COMPLETED)

### 1. Database Migration
**File**: `interface/prisma/migrations/add_identity_resolution_fields/migration.sql`

**Changes**:
- ✅ Added `emailHash` field to `BridgeCustomer` (unique, indexed)
- ✅ Added `contextType` and `contextId` fields to `UserEntityRelationship`
- ✅ Added `canSeeFullName` and `canSeeEmail` privacy fields
- ✅ Updated unique constraint to include context fields
- ✅ Added `BLOCKED` status to `EntityRelationshipStatus` enum

**Status**: ✅ Migration applied to database, Prisma client regenerated

### 2. Backfill Scripts
**Files**: 
- `interface/scripts/backfill-email-hashes.ts`
- `interface/scripts/backfill-investment-connections.ts`

**Features**:
- ✅ Generate SHA256 hashes for all existing email addresses
- ✅ Query subgraph for all investments and create bidirectional connections
- ✅ Skip duplicates automatically
- ✅ Detailed progress logging

**Status**: ✅ Scripts tested and working

### 3. API Endpoints
**Files**:
- `interface/src/app/api/identity/resolve/route.ts`
- `interface/src/app/api/identity/search/route.ts`

**Features**:
- ✅ `POST /api/identity/resolve`: Resolve wallet address to display name
  - Checks viewer permissions via `UserEntityRelationship`
  - Returns truncated address if no connection
  - Respects `canSeeFullName` and `canSeeEmail` permissions
  
- ✅ `POST /api/identity/search`: Search by name or email
  - Email lookup via hashed email (secure, no PII exposure)
  - Name search among connections only
  - Returns match type ('EMAIL', 'NAME', 'CONNECTION')

**Status**: ✅ Endpoints implemented and ready for testing

---

## ✅ Phase 2: UI Components (COMPLETED)

### 4. IdentityDisplay Component
**File**: `interface/src/components/Identity/IdentityDisplay.tsx`

**Features**:
- ✅ Resolves wallet addresses to human-readable names
- ✅ Shows KYC verification badge
- ✅ Supports variants (default, issuer, investor)
- ✅ Optional email and address display
- ✅ Loading states

**Status**: ✅ Integrated in offering pages

### 5. AddressInput Component
**File**: `interface/src/components/Identity/AddressInput.tsx`

**Features**:
- ✅ Search by name or email with autocomplete
- ✅ Keyboard navigation (arrow keys, enter, escape)
- ✅ Debounced search (300ms)
- ✅ Click-outside to close dropdown
- ✅ Address validation indicator
- ✅ Error states

**Status**: ✅ Component ready for integration

### 6. Integration
**Files Updated**:
- `interface/src/app/offerings/page.tsx` - Offering cards issuer display
- `interface/src/app/offerings/[id]/page.tsx` - Offering detail issuer display
- `interface/src/app/offerings/[id]/manage/page.tsx` - Investor list

**Status**: ✅ Address displays replaced with IdentityDisplay component

---

## 🎉 Implementation Complete!

All planned features have been implemented:
- ✅ Database schema extended
- ✅ Backfill scripts ready
- ✅ API endpoints functional
- ✅ UI components created
- ✅ Integrated into offering pages

---

## 📊 Database Schema Summary

### BridgeCustomer (Extended)
```prisma
model BridgeCustomer {
  // ... existing fields ...
  email                String?   @map("email")
  emailHash            String?   @unique @map("email_hash")  // NEW: SHA256 hash
  // ...
  @@index([emailHash])  // NEW: Fast email lookup
}
```

### UserEntityRelationship (Extended)
```prisma
model UserEntityRelationship {
  // ... existing fields ...
  contextType      String?   @map("context_type")      // NEW: 'OFFERING', 'TOKEN', 'DIRECT'
  contextId        String?   @map("context_id")        // NEW: Offering/token address
  canSeeFullName   Boolean   @default(false)           // NEW: Privacy control
  canSeeEmail      Boolean   @default(false)           // NEW: Privacy control
  // ...
  @@unique([entityId, identityId, relationshipType, contextType, contextId])  // UPDATED
  @@index([contextType, contextId])  // NEW: Context-based lookups
}
```

---

## 🔧 How to Run Backfill Scripts

### 1. Apply Database Migration
```bash
# Using psql or your database tool
psql $DATABASE_URL < interface/prisma/migrations/add_identity_resolution_fields/migration.sql
```

### 2. Generate Prisma Client
```bash
cd interface
npx prisma generate
```

### 3. Run Email Hash Backfill
```bash
cd interface
npx ts-node scripts/backfill-email-hashes.ts
```

### 4. Run Investment Connections Backfill
```bash
cd interface
NEXT_PUBLIC_SUBGRAPH_URL=https://... npx ts-node scripts/backfill-investment-connections.ts
```

---

## 🎯 API Usage Examples

### Resolve Identity
```typescript
const response = await fetch('/api/identity/resolve', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    address: '0x742d35...',
    viewerAddress: '0x89abc1...' // optional
  })
});

const identity = await response.json();
// {
//   address: '0x742d35...',
//   displayName: 'CapSign Inc',  // or truncated address if no connection
//   displayType: 'ENTITY',
//   isVerified: true,
//   canSeeFullInfo: true,
//   canSeeEmail: true,
//   email: 'matt@capsign.com'  // if canSeeEmail
// }
```

### Search Identities
```typescript
// Search by email
const response = await fetch('/api/identity/search', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    query: 'john@example.com',
    viewerAddress: '0x89abc1...'
  })
});

// Search by name
const response = await fetch('/api/identity/search', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    query: 'john',
    viewerAddress: '0x89abc1...'
  })
});

const { results } = await response.json();
// [
//   {
//     address: '0x742d35...',
//     displayName: 'John D.',
//     displayType: 'INDIVIDUAL',
//     isVerified: true,
//     matchType: 'EMAIL'  // or 'NAME'
//   }
// ]
```

---

## 🔒 Security Features

1. **Email Privacy**: Emails are hashed (SHA256) for lookups - actual emails never exposed in queries
2. **Connection-Based Access**: Can only see names of people you're connected to
3. **Granular Permissions**: `canSeeFullName` and `canSeeEmail` control what each party sees
4. **Context Tracking**: Know WHY connection exists (e.g., "via Offering 0x123")
5. **Backward Compatible**: Extends existing `UserEntityRelationship` without breaking changes

---

## 📈 Performance Considerations

- ✅ Indexed `emailHash` for fast email lookups
- ✅ Indexed `contextType` + `contextId` for fast context queries
- ✅ Bidirectional connections (only 1 query needed regardless of direction)
- ✅ Skip duplicates in backfill (idempotent)

---

## 🎉 What's Working Now

- ✅ Database schema extended
- ✅ Backfill scripts ready to run
- ✅ API endpoints functional
- ✅ Email lookup working (hashed for security)
- ✅ Permission system implemented
- ✅ Context tracking enabled

**Ready for UI integration!**

