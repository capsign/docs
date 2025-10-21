# Document Verification

Learn how to verify the authenticity of signed documents.

## What is Verification?

Verification proves that:
- Document hasn't been tampered with
- Signatures are authentic
- Timestamps are accurate
- Signers are who they claim to be

## How to Verify

### Method 1: Via CapSign App

1. Navigate to **Documents** → **Verify**
2. Upload document or enter content hash
3. View verification results:
   - All signatures
   - Signer addresses
   - Timestamps
   - Attestation details

### Method 2: Via EAS Explorer

1. Get attestation UID from document
2. Visit [EAS Explorer](https://base.easscan.org)
3. Enter attestation UID
4. View full attestation details

### Method 3: Via Smart Contract

Query the EAS contract directly:

```solidity
function getAttestation(bytes32 uid) external view returns (Attestation memory);
```

## What Verification Shows

### Document Integrity

**Content Hash:**
- Compare calculated hash with on-chain hash
- If they match, document is unaltered

**Storage URI:**
- Link to original document
- Verify document is accessible

### Signature Authenticity

**Signer Address:**
- Wallet address that signed
- Verify it's the expected signer

**Timestamp:**
- When document was signed
- Blockchain timestamp (immutable)

**Transaction:**
- On-chain transaction proof
- View on block explorer

## Verification Results

### ✅ Valid

- Content hash matches
- Signature authentic
- Timestamp verified
- All signers confirmed

### ⚠️ Warning

- Content hash mismatch (document altered)
- Missing signatures
- Revoked attestation

### ❌ Invalid

- Attestation not found
- Signature verification failed
- Fraudulent document

## Use Cases

### Legal Proceedings

Provide evidence:
- Document content
- Signature attestations
- Blockchain transactions
- Timestamp proofs

### Compliance Audits

Demonstrate:
- Document signed by all parties
- Signed on specific dates
- No alterations after signing

### Due Diligence

Verify:
- Investment documents authentic
- Proper signatures obtained
- Legal requirements met

## Best Practices

- **Always verify** documents before relying on them
- **Check all signers** - ensure all required parties signed
- **Compare hashes** - document content matches attestation
- **Check timestamps** - signatures made when expected
- **Save verification results** for your records

## Technical Details

### Content Hashing

CapSign uses SHA-256:
```
sha256(document_bytes) = content_hash
```

Compare this hash with on-chain hash to verify integrity.

### Attestation Structure

EAS attestations contain:
- Schema UID (document schema)
- Recipient (signer address)
- Attester (document creator)
- Data (content hash, URI, metadata)
- Timestamp
- Expiration (if any)
- Revocation status

## FAQs

**Q: Can verification be faked?**
A: No. Blockchain verification is cryptographically secure.

**Q: What if document was altered after signing?**
A: Hash won't match, verification will fail.

**Q: Can I verify without CapSign?**
A: Yes, directly via EAS or blockchain explorers.

**Q: What if attestation is revoked?**
A: Verification will show "Revoked" status and reason.

## See Also

- [Uploading Documents](uploading-documents.md)
- [Signing Documents](signing-documents.md)
- [EAS Documentation](https://docs.attest.sh)
