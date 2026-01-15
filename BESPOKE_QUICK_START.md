# Bespoke Documents - Quick Start Guide

## What Changed?

### ✅ Template Creation Flow
- **New Step 0**: Choose between "Offering Template" or "Bespoke Agreement"
- **Offering templates**: Investment documents with automatic signer qualification
- **Bespoke templates**: Custom contracts where you invite specific signers

### ✅ Template Types Added
```typescript
enum TemplateType {
  OFFERING  // Pull model - investors self-qualify
  BESPOKE   // Push model - creator invites signers
}
```

### ✅ On-Chain Registration
Bespoke documents are automatically:
1. Uploaded to Vercel Blob storage
2. Registered on your wallet's `WalletDocumentsFacet`
3. Indexed by the subgraph for discovery

### ✅ EIP-712 Signatures
Bespoke documents use cryptographic signatures:
- Domain: "CapSign Documents"
- Verified on-chain by `WalletDocumentsFacet`
- Stored in database with transaction hash

### ✅ Document Discovery
Documents appear in `/documents` from three sources:
1. **Database** - Off-chain drafts
2. **Offering Documents** - From `OfferingDocumentsFacet` (via subgraph)
3. **Bespoke Documents** - From `WalletDocumentsFacet` (via subgraph)

## How to Use

### Creating a Bespoke NDA

1. Go to `/documents/templates/create`
2. Select **"Bespoke Agreement"**
3. Choose agreement type: **NDA**
4. Click **"HTML Source"** and paste your HTML contract
5. Add signer roles: `["Disclosing Party", "Receiving Party"]`
6. Create template

### Using the Template

1. Go to `/documents/templates`
2. Find your template and click **"Use Template"**
3. Fill in placeholders (company names, dates, etc.)
4. Enter signer emails:
   - Disclosing Party: `alice@company.com`
   - Receiving Party: `bob@othercompany.com`
5. Click **"Create & Preview Document"**

### What Happens Next

**Automatically:**
1. ✅ HTML uploaded to Vercel Blob
2. ✅ Emails resolved to wallet addresses
3. ✅ Document registered on your wallet's blockchain
4. ✅ Database updated with on-chain details
5. ✅ Subgraph indexes the document
6. ✅ Signers can see it in their "Documents" page

**Alice and Bob will:**
1. See the document in `/documents` (via subgraph query)
2. Click **"Sign"** to review the document
3. Generate EIP-712 signature
4. Submit signature to blockchain
5. Document shows as "Signed" after all parties sign

## Database Migration

Run this to apply schema changes:

```bash
cd /Users/matt/Desktop/capsign/interface
npx prisma migrate dev
```

This adds:
- `TemplateType` enum
- `templateType` field to templates
- On-chain fields to documents (`documentHash`, `contentHash`, etc.)

## Key Differences: Offering vs Bespoke

| Feature | Offering Templates | Bespoke Templates |
|---------|-------------------|-------------------|
| **Model** | Pull (investor-driven) | Push (creator-driven) |
| **Signer Selection** | Automatic (offering requirements) | Manual (email invitation) |
| **Storage Contract** | OfferingDocumentsFacet | WalletDocumentsFacet |
| **Use Case** | Investment docs (SAFEs, subs) | Custom contracts (NDAs, etc.) |
| **Signer Discovery** | Via offering eligibility | Via email → wallet resolution |
| **Representative Signing** | Yes (entity representatives) | No (direct signers only) |

## Technical Details

### Storage Architecture
```
Creator's Wallet (Diamond)
  └─ WalletDocumentsFacet
       └─ Documents Storage
            ├─ documentId (bytes32)
            ├─ contentHash (bytes32)
            ├─ storageURI (string) → Vercel Blob
            ├─ requiredSigners (address[])
            └─ signatures (mapping)
```

### Signature Verification
```
EIP-712 Domain:
  name: "CapSign Documents"
  version: "1"
  chainId: <current chain>
  verifyingContract: <creator's wallet>

Message Type:
  DocumentSignature(
    bytes32 documentId,
    bytes32 contentHash,
    address signer,
    uint256 timestamp
  )
```

### Identity Resolution
```
Email → Account → Wallet Address

1. Hash email with SHA-256
2. Lookup in bridgeCustomer.emailHash
3. Get account.wallet.address
4. Return wallet address for on-chain registration
```

## Troubleshooting

### Signer Not Found
**Problem:** Email doesn't resolve to wallet address  
**Solution:** User must have a CapSign account with verified email

### Document Not Appearing in Subgraph
**Problem:** Document created but not visible in `/documents`  
**Solution:** Wait for subgraph to index (~30 seconds). Check block explorer for transaction.

### Signature Failed
**Problem:** `WalletDocuments_NotAuthorizedSigner` error  
**Solution:** Verify signer's wallet address matches one in `requiredSigners` array

## Next Steps

1. ✅ Run database migration
2. ✅ Test template creation (both types)
3. ✅ Test bespoke document flow end-to-end
4. ✅ Verify subgraph indexing
5. 🔄 Add email notifications (future)
6. 🔄 Add in-app notifications (future)
