# Executive Officer Document Signing

## ✅ Approved Simplified Implementation

**Authorization:** Off-chain (UserEntityRelationship-based)  
**Backwards Compatibility:** Not required (clean migration)  
**Authorized Titles:** CEO, CFO, President, Secretary, Director  
**Representative Selection:** Optional but encouraged in templates

---

## Problem Statement

Currently, when an entity countersigns investment documents in offerings, the signature is recorded as coming from the **entity's smart wallet address**. This creates several issues:

1. **Downloaded documents lack human names** - Signature blocks show wallet addresses instead of "John Doe, CEO"
2. **No accountability** - Can't determine which executive officer actually signed
3. **Legal compliance** - Corporate documents typically require an authorized officer's signature
4. **UX friction** - Executive must use entity's EOA/multisig instead of their personal passkey

## Current Architecture

### Protocol Layer
```solidity
// OfferingCoreFacet.sol - Line 164
function countersignInvestment(uint256 investmentId) external {
    if (!_isAuthorizedForIssuer()) revert OfferingCore_NotAuthorized();
    
    // Signs as msg.sender (entity smart wallet)
    if (investment.subscriptionAgreementRef != bytes32(0)) {
        _signInvestmentDocument(investment.subscriptionAgreementRef);
    }
    // ...
}

// Line 827
function _signInvestmentDocument(bytes32 documentId) internal {
    // Records ONLY msg.sender (entity wallet)
    s.documentSignatures[documentId][msg.sender] = block.timestamp;
    emit DocumentSigned(documentId, msg.sender, block.timestamp);
}
```

**Access Control:**
- `_isAuthorizedForIssuer()` checks if `msg.sender == issuer`
- OR if issuer is a contract, calls `isDelegateAuthorized(msg.sender)`
- This allows delegation but doesn't track WHO delegated

### Database Layer
```prisma
model Signature {
  signerAccountId String? // Only stores ONE signer
  signerAddress   String? // Entity wallet address
  signerRole      String? // Template role (e.g., "Company Representative")
  // Missing: actual human who signed
}
```

### Frontend Layer
```typescript
// User in entity context calls countersignAA()
// Uses entity's smart account, not individual's passkey
const userOp = await countersignAA({
  offeringAddress,
  investmentId,
  smartAccount: entitySmartAccount, // ❌ Entity wallet, not individual
  bundlerClient,
});
```

## Proposed Solution

### Architecture Overview

**Dual-Signature Tracking:**
- `actualSigner` - The individual executive's wallet (passkey)
- `onBehalfOf` - The entity they represent

**Flow:**
1. Individual user in entity context on offering management page
2. Clicks "Countersign Investment"
3. System prompts their **individual passkey wallet** (not entity wallet)
4. Individual signs with their passkey
5. Protocol records BOTH the individual's address AND the entity address
6. Downloaded documents show: "John Doe, CEO, signing on behalf of Acme Corp"

---

## Implementation Plan

### 1. Protocol Changes

#### A. Offering Storage - Track Both Signers

**File:** `protocol/packages/offerings/src/OfferingStorage.sol`

```solidity
struct DocumentSignature {
    address actualSigner;      // Individual who physically signed
    address onBehalfOf;        // Entity they represent (if applicable)
    uint256 timestamp;
    bool isDelegated;          // True if signing on behalf of entity
}

// Replace simple mapping with struct mapping
mapping(bytes32 => mapping(address => DocumentSignature)) public documentSignatures;
```

#### B. New Countersign Function with Representative Signing

**File:** `protocol/packages/offerings/src/facets/OfferingCoreFacet.sol`

```solidity
/**
 * @notice Countersign investment as an authorized representative of the issuer
 * @dev Authorization checked off-chain via UserEntityRelationship
 * @param investmentId Investment ID to countersign
 * @param entityAddress The entity address being represented (must be offering issuer)
 */
function countersignInvestmentAsRepresentative(
    uint256 investmentId,
    address entityAddress
) external nonReentrant {
    OfferingStorageLayout storage s = _getOfferingStorage();
    
    // Verify entity is the issuer
    if (entityAddress != s.info.issuer) {
        revert OfferingCore_NotIssuer();
    }
    
    // Note: Authorization is handled off-chain via UserEntityRelationship
    // Frontend/API verify the individual has proper officer title before tx
    
    Investment storage investment = s.investments[investmentId];
    if (investment.investor == address(0)) revert OfferingCore_InvestmentNotFound();
    if (investment.status != InvestmentStatus.PENDING) revert OfferingCore_AlreadyProcessed();
    
    // Execute hooks
    _executeBeforeCountersignHooks(investment.investor, investment.amount, investmentId);
    
    bool isFirstAcceptedInvestment = !_hasAcceptedInvestment(investment.investor);
    
    // Sign document WITH representative tracking
    if (investment.subscriptionAgreementRef != bytes32(0)) {
        _signInvestmentDocumentAsRepresentative(
            investment.subscriptionAgreementRef,
            msg.sender,      // actual signer (individual)
            entityAddress    // on behalf of (entity)
        );
    }
    
    // Transfer payment and issue tokens (entity has authority)
    _transferEscrowedPayment(investment, investmentId);
    
    investment.status = InvestmentStatus.ACCEPTED;
    investment.paymentEscrowed = false;
    _updateOfferingTotals(investment.amount, isFirstAcceptedInvestment);
    
    _executeAfterCountersignHooks(investment.investor, investment.amount, investmentId);
    
    // Emit with both addresses
    emit InvestmentAcceptedByRepresentative(
        investmentId,
        msg.sender,      // individual signer
        entityAddress,   // entity
        investment.investor
    );
}

/**
 * @notice Sign investment document as representative
 */
function _signInvestmentDocumentAsRepresentative(
    bytes32 documentId,
    address actualSigner,
    address entity
) internal {
    OfferingStorageLayout storage s = _getOfferingStorage();
    
    if (documentId == bytes32(0)) return;
    
    // Check if entity already signed
    DocumentSignature storage sig = s.documentSignatures[documentId][entity];
    if (sig.timestamp != 0) return; // Already signed
    
    // Record dual signature
    sig.actualSigner = actualSigner;
    sig.onBehalfOf = entity;
    sig.timestamp = block.timestamp;
    sig.isDelegated = true;
    
    emit DocumentSignedByRepresentative(
        documentId,
        actualSigner,
        entity,
        block.timestamp
    );
}

```

#### C. No Additional Wallet Facets Needed! ✅

Off-chain authorization means we don't need to add any new facets to entity wallets. The existing offering authorization (`_isAuthorizedForIssuer`) already verifies the entity is the issuer - we just track WHO from that entity actually signed.

---

### 2. Database Changes

**File:** `interface/prisma/schema.prisma`

```prisma
model Signature {
  id              String  @id @default(cuid())
  documentId      String
  
  // Dual signature tracking
  actualSignerAccountId  String?  // Individual who signed
  onBehalfOfAccountId    String?  // Entity they represent
  isDelegated            Boolean  @default(false)
  
  // Legacy fields (keep for backwards compatibility)
  signerAccountId String? // Deprecated, use actualSignerAccountId
  signerAddress   String?
  
  // Template and blockchain fields
  signerRole      String?
  signatureData   Bytes?
  signedAt        DateTime?
  status          SignatureStatus?
  blockNumber     BigInt?
  transactionId   String?
  
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  
  document         Document @relation(fields: [documentId], references: [id])
  actualSigner     Account? @relation("ActualSigner", fields: [actualSignerAccountId], references: [id])
  onBehalfOfEntity Account? @relation("OnBehalfOf", fields: [onBehalfOfAccountId], references: [id])
  
  @@index([actualSignerAccountId])
  @@index([onBehalfOfAccountId])
}

// Update Account to support new relations
model Account {
  // ... existing fields ...
  
  signaturesAsSigner       Signature[] @relation("ActualSigner")
  signaturesOnBehalfOf     Signature[] @relation("OnBehalfOf")
}
```

**Migration:**
```sql
-- Add new columns
ALTER TABLE "signatures" ADD COLUMN "actual_signer_account_id" TEXT;
ALTER TABLE "signatures" ADD COLUMN "on_behalf_of_account_id" TEXT;
ALTER TABLE "signatures" ADD COLUMN "is_delegated" BOOLEAN DEFAULT false;

-- Add indexes
CREATE INDEX "signatures_actual_signer_account_id_idx" ON "signatures"("actual_signer_account_id");
CREATE INDEX "signatures_on_behalf_of_account_id_idx" ON "signatures"("on_behalf_of_account_id");

-- Migrate existing data (all are direct signatures)
UPDATE "signatures" 
SET "actual_signer_account_id" = "signer_account_id",
    "is_delegated" = false
WHERE "signer_account_id" IS NOT NULL;
```

---

### 3. Frontend Changes

#### A. AA Library - Representative Signing

**File:** `interface/src/lib/aa/offering-operations.ts`

```typescript
/**
 * Countersign investment as an authorized representative
 * Uses individual's passkey wallet to sign on behalf of entity
 */
export async function countersignAsRepresentativeAA({
  offeringAddress,
  investmentId,
  entityAddress,
  individualSmartAccount, // Individual's passkey wallet
  bundlerClient,
}: {
  offeringAddress: Address;
  investmentId: bigint;
  entityAddress: Address;
  individualSmartAccount: SmartAccountClient;
  bundlerClient: BundlerClient;
}): Promise<Hash> {
  // Call the new countersignInvestmentAsRepresentative function
  const userOpHash = await individualSmartAccount.sendUserOperation({
    calls: [
      {
        to: offeringAddress,
        data: encodeFunctionData({
          abi: OFFERING_ABI,
          functionName: "countersignInvestmentAsRepresentative",
          args: [investmentId, entityAddress],
        }),
      },
    ],
  });

  return userOpHash;
}
```

#### B. Offering Management Page

**File:** `interface/src/app/offerings/[id]/manage/page.tsx`

```typescript
const handleCountersign = async (investmentId: string) => {
  const isEntityContext = session?.user?.context?.type === "entity";
  
  if (isEntityContext) {
    // Get individual's passkey wallet
    const individualWallet = await getIndividualSmartAccount();
    const entityAddress = session.user.context.wallet.address;
    
    // Verify individual has permission to sign for entity
    const hasPermission = await checkRepresentativePermission(
      individualWallet.address,
      entityAddress
    );
    
    if (!hasPermission) {
      toast.error("You don't have permission to sign on behalf of this entity");
      return;
    }
    
    // Sign with individual's passkey wallet
    const userOp = await countersignAsRepresentativeAA({
      offeringAddress: id as Address,
      investmentId: BigInt(investmentId),
      entityAddress: entityAddress as Address,
      individualSmartAccount: individualWallet,
      bundlerClient,
    });
    
    toast.success("Investment countersigned by you on behalf of the entity");
  } else {
    // Individual context - sign directly
    const userOp = await countersignAA({
      offeringAddress: id as Address,
      investmentId: BigInt(investmentId),
      smartAccount: await smartAccount,
      bundlerClient,
    });
  }
};
```

#### C. Permission Check Helper

**File:** `interface/src/lib/permissions/representative.ts` (NEW)

```typescript
/**
 * Check if an individual can sign on behalf of an entity
 */
export async function checkRepresentativePermission(
  individualAddress: Address,
  entityAddress: Address
): Promise<boolean> {
  // Query UserEntityRelationship from database
  const response = await fetch(
    `/api/permissions/representative?individual=${individualAddress}&entity=${entityAddress}`
  );
  
  const data = await response.json();
  return data.isAuthorized;
}
```

**File:** `interface/src/app/api/permissions/representative/route.ts` (NEW)

```typescript
export async function GET(request: NextRequest) {
  const { searchParams } = new URL(request.url);
  const individualAddr = searchParams.get("individual");
  const entityAddr = searchParams.get("entity");
  
  // Get individual's account
  const individualAccount = await prisma.account.findFirst({
    where: {
      wallet: { address: individualAddr?.toLowerCase() },
    },
  });
  
  // Get entity's account
  const entityAccount = await prisma.account.findFirst({
    where: {
      wallet: { address: entityAddr?.toLowerCase() },
    },
  });
  
  if (!individualAccount || !entityAccount) {
    return NextResponse.json({ isAuthorized: false });
  }
  
  // Check UserEntityRelationship with appropriate role
  const relationship = await prisma.userEntityRelationship.findFirst({
    where: {
      identityId: individualAccount.id,
      entityId: entityAccount.id,
      status: "ACTIVE",
      title: {
        in: ["CEO", "CFO", "President", "Secretary", "Director"], // Officer titles
      },
    },
  });
  
  return NextResponse.json({
    isAuthorized: !!relationship,
    title: relationship?.title,
  });
}
```

---

### 4. Document Download Enhancement

**File:** `interface/src/app/api/documents/[id]/download/route.ts`

```typescript
// Fetch signatures with BOTH actual signer and entity
const signatures = await prisma.signature.findMany({
  where: { documentId: id },
  include: {
    actualSigner: {
      include: {
        individualProfile: true,
        managedBy: {
          where: { status: "ACTIVE" },
        },
      },
    },
    onBehalfOfEntity: {
      include: {
        entityProfile: true,
        address: true,
      },
    },
  },
});

// Generate signature block
signatures.map((sig) => {
  if (sig.isDelegated && sig.actualSigner && sig.onBehalfOfEntity) {
    const signerName = sig.actualSigner.individualProfile?.displayName;
    const entityName = sig.onBehalfOfEntity.entityProfile?.displayName;
    const title = sig.actualSigner.managedBy[0]?.title;
    
    return `
      <div class="signature-box">
        <h3>Company Representative</h3>
        <div class="signature-field">
          <strong>Signed By:</strong> ${signerName}
        </div>
        <div class="signature-field">
          <strong>Title:</strong> ${title}
        </div>
        <div class="signature-field">
          <strong>On Behalf Of:</strong> ${entityName}
        </div>
        <div class="signature-field">
          <strong>Signed At:</strong> ${new Date(sig.signedAt).toLocaleString()}
        </div>
      </div>
    `;
  } else {
    // Regular signature...
  }
});
```

---

## ✅ Access Control: On-Chain via Entity Wallet AccessControl (SECURE)

**Implementation:**
1. Entity wallet has `AccessControlFacet` (already exists on all entity wallets)
2. Offering has configurable `documentSignerRoleId` (defaults to 50)
3. Grant role to authorized officers (CEO, CFO, President, Secretary, Director)
4. Protocol calls entity wallet's `hasRole(individual, roleId)` to verify authority
5. Off-chain: UserEntityRelationship + UI also checks for better UX
6. Offering admin can call `setDocumentSignerRole(roleId)` to customize

**Why On-Chain is Better:**
- ✅ **Trustless** - Cannot be bypassed by calling contract directly
- ✅ **Central source of truth** - Entity wallet controls its own authorized representatives
- ✅ **Already exists** - Leverages existing AccessControlFacet on entity wallets
- ✅ **Follows AccessManager pattern** - Other contracts read permissions from entity
- ✅ **Immutable audit trail** - Role grants/revokes recorded on-chain

**Security Model:**
```
Frontend → Check UserEntityRelationship → Show button (UX optimization)
                ↓
User calls → countersignAsRepresentative()
                ↓
Protocol → Verifies entityAddress == offering.issuer
                ↓
Protocol → Calls entity.hasRole(msg.sender, DOCUMENT_SIGNER_ROLE)
                ↓
If false → Revert OfferingCore_NotAuthorizedRepresentative
If true → Sign document with dual tracking
```

**Role Management:**
- Default: Role 50 = DOCUMENT_SIGNER_ROLE
- Configurable per offering via `setDocumentSignerRole(roleId)`
- Granted via entity wallet's AccessControl
- Can be managed through entity admin UI
- Synced with UserEntityRelationship for UX (off-chain db tracks title)

**Flexibility:**
```solidity
// Set custom role ID for this offering (issuer/admin only)
offering.setDocumentSignerRole(55); // Use role 55 instead

// Query current role requirement
uint8 roleId = offering.getDocumentSignerRole(); // Returns 55

// Entity wallet grants role to officers
entity.grantRole(alice, 55); // Alice can now sign
```

---

## Migration Path

### Phase 1: Database & Backend (Week 1)
- [ ] Add Signature schema fields
- [ ] Create migration
- [ ] Update API routes
- [ ] Add permission check endpoint

### Phase 2: Protocol (Week 2)
- [ ] Add DocumentSignature struct
- [ ] Implement countersignInvestmentAsRepresentative
- [ ] Add wallet representative authorization
- [ ] Write tests
- [ ] Deploy to testnet

### Phase 3: Frontend (Week 3)
- [ ] Add countersignAsRepresentativeAA
- [ ] Update offering management page
- [ ] Add permission UI
- [ ] Update document download

### Phase 4: Integration & Testing (Week 4)
- [ ] End-to-end testing
- [ ] Fix bugs
- [ ] Deploy to mainnet
- [ ] Update documentation

---

## Security Considerations

1. **Representative Verification:**
   - MUST verify UserEntityRelationship before allowing signature
   - MUST check active status and appropriate title
   - MUST validate entity address matches offering issuer

2. **Replay Protection:**
   - Individual can't sign for multiple entities with same signature
   - Entity address is part of signed data

3. **Access Revocation:**
   - If UserEntityRelationship is revoked, can't sign anymore
   - Existing signatures remain valid (tamper-proof)

4. **Audit Trail:**
   - Track both individual AND entity in signatures
   - Immutable record of who signed when

---

## Alternative: Template-Based Approach (Simpler)

If protocol changes are too complex, we could:

1. Keep current protocol as-is
2. When filling templates, specify TWO signers:
   - "Company Representative" → Individual passkey
   - "Company" → Entity wallet (auto-signs after individual)
3. Both signatures recorded separately
4. Downloaded doc shows both

**Pros:** No protocol changes, works today
**Cons:** Two separate signatures, more gas, less elegant

---

## Recommendation

**Implement Full Solution** for these reasons:

1. **Legal Compliance**: Corporate docs need human signers
2. **Better UX**: Executives use their own passkeys
3. **Proper Attribution**: "John Doe, CEO" vs "0x1234..."
4. **Future-Proof**: Enables delegation workflows
5. **Security**: Tracked accountability

**Timeline:** 4 weeks for full implementation
**Effort:** Medium (protocol + database + frontend)
**Impact:** High (required for enterprise adoption)

