# Documents

CapSign provides blockchain-based document management with cryptographic signatures.

## What are CapSign Documents?

Documents on CapSign are:

- **Uploaded to decentralized storage** (IPFS/Arweave)
- **Signed with biometric authentication** (Face ID/Touch ID)
- **Recorded on-chain as attestations** (via Ethereum Attestation Service)
- **Verifiable by anyone** with the document hash

## Key Features

### Cryptographic Signatures

- **Tamper-proof** - Can't be altered after signing
- **Verifiable** - Anyone can verify authenticity
- **Legally binding** - Recognized in many jurisdictions
- **Timestamped** - Exact signing time recorded

### Biometric Authentication

Every signature requires Face ID or Touch ID - no one can sign on your behalf.

### Decentralized Storage

Documents stored on:
- **IPFS** - Distributed file storage
- **Arweave** - Permanent storage
- **Your choice** - Or use your own storage

### EAS Integration

Document signatures are EAS attestations:
- **Privacy-preserving** - Document content not on-chain
- **Portable** - Use attestations across platforms
- **Revocable** - Can be revoked if needed

## Document Types

Common document categories:

- **Subscription Agreements** - Investment contracts
- **Operating Agreements** - Company governance
- **Employment Contracts** - Hiring documents
- **NDAs** - Non-disclosure agreements
- **Consents** - Shareholder consents
- **Amendments** - Document amendments
- **Custom** - Any document type

## For Users

### Signing Documents

1. Navigate to **Documents**
2. Click on document to review
3. Read entire document
4. Click **Sign**
5. Authenticate with Face ID/Touch ID
6. Signature recorded on-chain

Learn more: [Signing Documents](signing-documents.md)

### Viewing Signed Documents

See all your signed documents:

- Navigate to **Documents** → **Signed**
- View document content
- See signature details
- Export signatures for records

### Verifying Signatures

To verify a document signature:

1. Navigate to **Documents** → **Verify**
2. Upload document or enter hash
3. See all signatures and signers
4. Verify authenticity

Learn more: [Document Verification](verification.md)

## For Issuers

### Uploading Documents

1. Navigate to **Documents** → **Upload**
2. Select file or enter IPFS/Arweave URL
3. Choose category
4. Add required signers (optional)
5. Upload

Learn more: [Uploading Documents](uploading-documents.md)

### Requesting Signatures

After uploading:

- Add required signers
- Send notification
- Track signing status
- Download signed copies

### Document Management

- Organize by category
- Search documents
- Export document lists
- Archive old documents

## Technical Details

### How It Works

1. **Upload** - Document uploaded to IPFS/Arweave
2. **Hash** - Content hash (SHA-256) calculated
3. **Attestation** - EAS attestation created with:
   - Content hash
   - Storage URI
   - Signer address
   - Timestamp
   - Category
4. **Signature** - User signs with smart wallet
5. **On-chain** - Attestation recorded

### What Goes On-Chain

Only the **attestation** goes on-chain:
- Content hash (not document itself)
- Signer address
- Timestamp
- Metadata (category, title)

**Document content stays off-chain** for privacy.

### Storage Options

- **IPFS** - Distributed, content-addressed
- **Arweave** - Permanent storage
- **Custom URL** - Host yourself

## Best Practices

### Before Signing

- **Read everything** - Don't sign without reading
- **Verify sender** - Confirm document is from legitimate source
- **Save copy** - Download document for your records
- **Understand terms** - Ask questions if unclear

### Document Organization

- **Use categories** - Organize documents logically
- **Descriptive titles** - Make documents easy to find
- **Regular backups** - Export document lists periodically
- **Secure storage** - Keep local copies securely

### Security

- **Verify authenticity** - Check document hasn't been tampered with
- **Check signers** - Ensure all required parties signed
- **Revoke if needed** - Revoke attestation if document is invalid
- **Report suspicious** - Contact support if something seems wrong

## Legal Considerations

### Electronic Signatures

CapSign signatures are electronic signatures under:
- **ESIGN Act** (US)
- **UETA** (US states)
- **eIDAS** (EU)

Legally binding in most jurisdictions, but consult local laws.

### Evidence Requirements

For legal proceedings, you may need:
- Document content
- Signature attestation
- Blockchain transaction
- Timestamp proof

CapSign provides all of this automatically.

## FAQs

**Q: Are CapSign signatures legally binding?**
A: Yes, in most jurisdictions under electronic signature laws. Consult local attorney.

**Q: Can I sign the same document multiple times?**
A: Yes, each signing creates a new attestation.

**Q: What if I signed by mistake?**
A: Contact the document owner to discuss. Signatures can't be deleted but attestations can be revoked.

**Q: Can others see my signed documents?**
A: Only if you share them. Documents are private by default.

**Q: What happens if IPFS/Arweave goes down?**
A: Keep local copies. The on-chain attestation proves you signed even if storage is unavailable.

## Guides

- [Uploading Documents](uploading-documents.md)
- [Signing Documents](signing-documents.md)
- [Document Verification](verification.md)

## Need Help?

- **Email:** support@capsign.com
- **Twitter:** [@CapSignInc](https://twitter.com/CapSignInc)
