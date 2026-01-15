# Bespoke Documents Feature

## Overview

The Bespoke Documents feature enables users to create custom bilateral or multilateral agreements (NDAs, service agreements, etc.) using a "push" model where the document creator explicitly invites signers by email.

This is distinct from Offering Templates which use a "pull" model where investors self-qualify based on offering requirements.

## Architecture

### Template Types

```
┌──────────────────────────────────────────────────────────────┐
│                     OFFERING TEMPLATES                        │
├──────────────────────────────────────────────────────────────┤
│ • Permission Model: PULL (Offering-based eligibility)        │
│ • Stored in: OfferingDocumentsFacet (on-chain)               │
│ • Signers: Automatically determined by offering requirements │
│ • Use Case: Investment documents (SAFEs, subscriptions)      │
└──────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────┐
│                     BESPOKE TEMPLATES                         │
├──────────────────────────────────────────────────────────────┤
│ • Permission Model: PUSH (Creator explicitly invites)        │
│ • Stored in: WalletDocumentsFacet (on-chain)                 │
│ • Signers: Explicitly defined by document creator            │
│ • Use Case: Custom contracts (NDAs, service agreements)      │
└──────────────────────────────────────────────────────────────┘
```

## User Flow

### 1. Create Template

**Route:** `/documents/templates/create`

**Steps:**
1. **Select Template Type** (Step 0)
   - Choose between "Offering Template" or "Bespoke Agreement"
   - Sets `templateType` field

2. **Define Content** (Step 1)
   - For Offering: Select document purpose, template variant
   - For Bespoke: Select agreement type (NDA, Service Agreement, etc.)
   - Toggle between Rich Text and HTML Source mode
   - Add placeholders using `{{placeholder}}` syntax

3. **Configure Signers** (Step 2)
   - Define signer roles (e.g., "Party A", "Party B")
   - For Offering only: Configure representative signing

4. **Review** (Step 3)
   - Preview tab: See rendered HTML with iframe isolation
   - Details tab: Review metadata

### 2. Fill Template & Create Document

**Route:** `/documents/templates/[id]/fill`

**Process:**
1. **Fill placeholders** - Enter values for template variables
2. **Specify signers** - Enter email address for each signer role
3. **Submit** - Creates document with on-chain registration

**For Bespoke Documents:**
```typescript
// 1. Upload HTML to Vercel Blob
const { url: storageUri, contentHash } = await uploadToBlob(filledHtml);

// 2. Generate document ID
const documentHash = keccak256(toBytes(`${templateId}-${timestamp}-${walletAddress}`));

// 3. Resolve signer emails → wallet addresses
const signerAddresses = await resolveSignerEmails(signerEmails);

// 4. Register on-chain via WalletDocumentsFacet
await walletDiamond.createDocument(
  documentHash,
  contentHash,
  storageUri,
  category,
  signerAddresses,
  title,
  parentDocumentId
);

// 5. Store in database with on-chain metadata
await prisma.document.create({
  data: {
    documentHash,
    contentHash,
    storageUri,
    onChainWallet: walletAddress,
    onChainTxHash: txHash,
    // ... other fields
  }
});
```

### 3. Sign Document

**Route:** `/documents/[id]/sign`

**For Bespoke Documents:**
```typescript
// 1. Generate EIP-712 signature
const signature = await walletClient.signTypedData({
  domain: {
    name: "CapSign Documents",
    version: "1",
    chainId: currentChainId,
    verifyingContract: documentCreatorWallet, // Creator's wallet
  },
  types: {
    DocumentSignature: [
      { name: "documentId", type: "bytes32" },
      { name: "contentHash", type: "bytes32" },
      { name: "signer", type: "address" },
      { name: "timestamp", type: "uint256" },
    ],
  },
  message: {
    documentId,
    contentHash,
    signer: walletAddress,
    timestamp,
  },
});

// 2. Submit signature on-chain
await walletDiamond.signDocument(documentHash, signature);

// 3. Update database
await prisma.signature.update({
  where: { id: signatureId },
  data: {
    status: 'SIGNED',
    signatureData: signature,
    txHash: txHash,
  }
});
```

### 4. Document Discovery

**Route:** `/documents`

**Data Sources:**
1. **Database** - Off-chain documents (drafts, non-registered docs)
2. **Subgraph** - On-chain documents from WalletDocumentsFacet

**Subgraph Query:**
```graphql
query GetUserBespokeDocuments($userAddress: Bytes!) {
  # Documents created by user
  created: documents(
    where: { creator: $userAddress }
    orderBy: createdAt
    orderDirection: desc
  ) {
    id
    wallet { id }
    title
    category
    signatures { signer, signedAt }
    # ... more fields
  }
  
  # Documents where user is a required signer
  requiredToSign: documents(
    where: { requiredSigners_contains: [$userAddress] }
    orderBy: createdAt
    orderDirection: desc
  ) {
    # ... same fields
  }
}
```

## Smart Contract Integration

### WalletDocumentsFacet

**Location:** `/protocol/packages/wallets/src/facets/WalletDocumentsFacet.sol`

**Key Functions:**

```solidity
// Create document
function createDocument(
  bytes32 documentId,
  bytes32 contentHash,
  string memory storageURI,
  string memory category,
  address[] memory requiredSigners,
  string memory title,
  bytes32 parentDocumentId
) external;

// Sign document with EIP-712 signature
function signDocument(
  bytes32 documentId,
  bytes memory signature
) external;

// Void document
function voidDocument(
  bytes32 documentId,
  string memory reason
) external;
```

**Events:**
- `DocumentCreated(bytes32 indexed documentId, address indexed creator, string category, address[] requiredSigners, string title)`
- `DocumentSigned(bytes32 indexed documentId, address indexed signer, uint256 timestamp)`
- `DocumentVoided(bytes32 indexed documentId, address indexed voidedBy, uint256 timestamp, string reason)`

## Database Schema

### DocumentTemplate

```prisma
model DocumentTemplate {
  // ... existing fields
  templateType TemplateType @default(BESPOKE) // NEW: OFFERING or BESPOKE
  
  @@index([templateType])
}

enum TemplateType {
  OFFERING // Pull model
  BESPOKE  // Push model
}
```

### Document

```prisma
model Document {
  // ... existing fields
  
  // On-chain registration (for bespoke documents)
  documentHash   Bytes?  // bytes32 from WalletDocumentsFacet
  contentHash    Bytes?  // keccak256 of HTML content
  storageUri     String? // Vercel Blob URL
  onChainWallet  String? // Creator's wallet address
  onChainTxHash  String? // Registration transaction
  
  @@index([documentHash])
  @@index([onChainWallet])
}
```

## Storage

### Vercel Blob

Documents are stored using Vercel Blob with the following path structure:

```
documents/{userId}/{timestamp}-{filename}.html
```

**Upload Endpoint:** `POST /api/documents/upload`

Accepts both:
- FormData with file
- JSON with `{ htmlContent, filename }`

Returns:
```json
{
  "url": "https://...",
  "contentHash": "0x...",
  "fileSize": 12345
}
```

## Goldsky Pipeline Integration

The subgraph indexes `WalletDocumentsFacet` events and makes them available via Goldsky:

**Indexed Events:**
- `DocumentCreated` → Creates `Document` entity
- `DocumentSigned` → Creates `DocumentSignature` entity
- `DocumentVoided` → Updates `Document` entity

**Entity Structure:**
```graphql
type Document @entity {
  id: ID!                      # bytes32 documentId
  wallet: Wallet!              # Creator's wallet
  contentHash: Bytes!
  storageURI: String!
  category: String!
  title: String!
  creator: Bytes!
  createdAt: BigInt!
  requiredSigners: [Bytes!]!
  signatures: [DocumentSignature!]!
  parentDocument: Document     # For template relationships
}

type DocumentSignature @entity {
  id: ID!                      # tx-hash-logIndex
  document: Document
  signer: Bytes!
  signedAt: BigInt!
  tx: Bytes!
  actualSigner: Bytes          # For representative signing
  onBehalfOf: Bytes            # For representative signing
  isDelegated: Boolean!
}
```

## API Endpoints

### Templates

- `POST /api/templates` - Create template (now requires `templateType`)
- `GET /api/templates?type=BESPOKE` - Filter by template type
- `GET /api/templates/[id]` - Get template details

### Documents

- `POST /api/documents` - Create document (accepts on-chain fields)
- `GET /api/documents` - List user's documents
- `POST /api/documents/upload` - Upload HTML to Vercel Blob
- `POST /api/documents/[id]/sign` - Sign document

### Identity

- `POST /api/identity/resolve` - Resolve email OR address to wallet
  ```json
  // By email
  { "email": "user@example.com" }
  
  // By address
  { "address": "0x..." }
  ```

## Security Considerations

### Access Control

1. **Document Viewing**
   - Only required signers can view bespoke documents
   - Enforced by checking `requiredSigners` array
   - Future: Add viewer permissions

2. **Signature Verification**
   - EIP-712 signatures verified on-chain
   - WalletDocumentsFacet validates signer is in `requiredSigners`
   - Database tracks signature status

3. **Document Voiding**
   - Only creator can void document
   - Voided documents cannot be signed
   - Permanent on-chain record

### Privacy

- Documents stored in Vercel Blob with public URLs
- Access control enforced at application layer
- TODO: Add signed URLs or gated access

## Testing Checklist

- [ ] Create OFFERING template
- [ ] Create BESPOKE template  
- [ ] Fill bespoke template with 2 signers
- [ ] Verify document uploaded to Vercel Blob
- [ ] Verify document registered on-chain (check event in block explorer)
- [ ] Verify document appears in subgraph
- [ ] Sign as first signer (EIP-712 signature)
- [ ] Verify signature appears on-chain
- [ ] Sign as second signer
- [ ] Verify document shows as fully signed
- [ ] Void a document
- [ ] Verify voided status on-chain

## Migration

Run the migration:
```bash
npx prisma migrate dev
```

This will:
1. Create `TemplateType` enum
2. Add `template_type` to `document_templates`
3. Add on-chain fields to `documents`
4. Create necessary indexes

## Future Enhancements

1. **Email Notifications** - Send email when user is added as required signer
2. **In-App Notifications** - Show pending signatures in notifications
3. **Document Versions** - Track document revisions
4. **Gated Blob Access** - Replace public URLs with signed URLs
5. **Bulk Signing** - Sign multiple documents at once
6. **Document Templates Marketplace** - Share templates (system templates only)
