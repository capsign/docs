# EIP-712 Document Signing Implementation Progress

## ✅ Completed

### 1. Protocol Updates (WalletDocumentsFacet)

**File**: `/protocol/packages/wallets/src/facets/WalletDocumentsFacet.sol`

**Added EIP-712 Support**:
- Domain separator with name "CapSign Documents", version "1"
- Typed data structure: `DocumentSignature(bytes32 documentId, bytes32 contentHash, address signer, uint256 timestamp)`
- New `signDocument()` function with cryptographic signature verification
- Maintained `signDocumentFor()` for backward compatibility (delegated signing)
- Added `verifyDocumentSignature()` for off-chain verification
- Added `getDocumentSignatureHash()` for frontend to construct signing messages
- Internal helper functions: `_domainSeparatorV4()`, `_hashTypedDataV4()`, `_recoverSigner()`

**File**: `/protocol/packages/wallets/interfaces/documents/IWalletDocuments.sol`

**Interface Updates**:
- Added `WalletDocuments_InvalidSignature` error
- Added `DocumentSignedWithSignature` event
- Added function signatures for `signDocument`, `verifyDocumentSignature`, and `getDocumentSignatureHash`

**Compilation Status**: ✅ Success (257 files compiled)

### 2. API Endpoints

**File**: `/interface/src/app/api/documents/[id]/register/route.ts`

**Endpoints**:
- `POST /api/documents/[id]/register` - Prepares document for on-chain registration
  - Computes content hash using `keccak256`
  - Generates document ID from document UUID
  - Returns registration data for frontend to submit via UserOperation
  
- `GET /api/documents/[id]/register?walletAddress=0x...` - Checks registration status
  - Queries wallet contract to verify if document exists on-chain
  - Returns on-chain data if registered

**Existing Endpoint** (already implemented):
- `POST /api/documents/[id]/sign` - Records signatures in database
  - Accepts `signerRole` and `signatureData`
  - Updates document status (PENDING_SIGNATURES → PARTIALLY_SIGNED → FULLY_SIGNED)

### 3. UI Fixes

**File**: `/interface/src/app/documents/[id]/sign/page.tsx`
- Added `decodeHtmlEntities()` helper to properly render HTML content
- Fixed HTML entity decoding (`&lt;` → `<`, `&gt;` → `>`)
- Removed `dark:prose-invert` class (using custom CSS)

**File**: `/interface/src/app/documents/templates/[id]/fill/page.tsx`
- Added HTML decoding for template preview
- Fixed placeholder extraction from encoded HTML

## 🚧 Next Steps

### 3. Frontend Document Signing Implementation

**What's needed**:
- Integrate with wallet (WebAuthn/EOA) for signing
- Construct EIP-712 typed data message
- Submit UserOperation to call `createDocument()` on wallet (registration)
- Submit UserOperation to call `signDocument()` with signature (signing)
- Display transaction status and confirmations

**Approach**:
1. Use existing `toCapSignSmartAccountWithEOA` or `toCapSignSmartAccountWithPasskey`
2. Create typed data using domain separator and struct hash
3. Sign with `signMessage` from smart account
4. Submit via bundler (already have infrastructure)

### 4. Signature Verification UI

**What's needed**:
- Display signature status (on-chain verification)
- Show signer details (address, timestamp)
- Verify button to check signature validity
- Visual indicators for signed vs pending

### 5. Offering Integration

**What's needed**:
- Link documents to offerings (via `relatedOfferingId`)
- Use `OfferingDocumentsFacet.setRequiredDocument()` for investment documents
- Compliance checks verify `hasSignedRequiredDocument()`
- Auto-create documents when accepting investments

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                        Frontend                              │
├─────────────────────────────────────────────────────────────┤
│  1. User fills template                                      │
│  2. Document created in database                             │
│  3. Call /api/documents/[id]/register (get registration data)│
│  4. Submit UserOp: wallet.createDocument(...)                │
│  5. Get EIP-712 hash: wallet.getDocumentSignatureHash(...)   │
│  6. User signs with WebAuthn/EOA                             │
│  7. Submit UserOp: wallet.signDocument(documentId, signature)│
│  8. Signature recorded on-chain + database                   │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                   Smart Contract Layer                       │
├─────────────────────────────────────────────────────────────┤
│  WalletDocumentsFacet (in Wallet Diamond)                    │
│  ├─ createDocument(): Store doc metadata on-chain            │
│  ├─ signDocument(): Verify & record EIP-712 signature        │
│  ├─ verifyDocumentSignature(): Check signature validity      │
│  └─ getDocument(): Query document data                       │
│                                                              │
│  OfferingDocumentsFacet (in Offering Diamond)                │
│  ├─ setRequiredDocument(): Link doc to offering              │
│  ├─ signDocument(): Delegate to wallet.signDocumentFor()     │
│  └─ hasSignedRequiredDocument(): Check compliance            │
└─────────────────────────────────────────────────────────────┘
```

## Benefits of EIP-712

1. **User Experience**: Wallets display structured, human-readable data
2. **Security**: Users see exactly what they're signing
3. **Replay Protection**: Domain separator prevents cross-contract replay
4. **Standard Compliance**: Works with all EIP-712 compatible wallets
5. **Verification**: Anyone can verify signatures off-chain
6. **Compatibility**: Existing `signDocumentFor()` still works for delegated signing

## Technical Details

**EIP-712 Domain**:
```
name: "CapSign Documents"
version: "1"
chainId: 84532 (Base Sepolia) / 8453 (Base)
verifyingContract: <wallet address>
```

**Typed Data Structure**:
```
DocumentSignature {
  bytes32 documentId;
  bytes32 contentHash;
  address signer;
  uint256 timestamp;
}
```

**Signature Format**: 65 bytes (r, s, v) standard ECDSA or WebAuthn



