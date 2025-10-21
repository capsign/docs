# Viewing Attestations

Learn how to view, manage, and understand your attestations on CapSign.

## What Are Attestations?

**Attestations** are blockchain-based credentials that prove facts about you without revealing personal information:

- **Identity Verification** - Proves you completed KYC
- **Accredited Investor** - Proves investment qualification
- **Age Verification** - Proves you meet age requirements
- **Geographic Location** - Proves your jurisdiction
- **Professional Credentials** - Proves qualifications
- **Custom Attestations** - Application-specific credentials

Learn more: [Key Concepts - Attestations](/getting-started/key-concepts.md#attestations)

## Benefits of Attestations

### Privacy-Preserving

- Prove facts without revealing personal data
- "I'm over 18" without revealing birthdate
- "I'm accredited" without revealing financial details
- Zero-knowledge proofs

### Portable

- Use across multiple applications
- No need to verify identity repeatedly
- Work with any EAS-compatible platform
- Follow your wallet address

### Verifiable

- Anyone can verify authenticity
- Check issuer and timestamp
- Confirm not revoked
- Transparent audit trail

### Permanent (But Revocable)

- Stored on blockchain immutably
- Can't be tampered with
- Issuer can revoke if needed
- Real-time verification

## Accessing Your Attestations

### Dashboard View

1. Log in to CapSign
2. Your dashboard shows:
   - **Active Attestations**: Currently valid
   - **Pending**: Under review
   - **Expired**: Need renewal
   - **Revoked**: No longer valid

### Detailed Attestations Page

Navigate to **Attestations** for full view:

1. All attestations listed
2. Filter by:
   - Type (KYC, accreditation, etc.)
   - Status (active, expired, revoked)
   - Issuer
   - Date range

3. Sort by:
   - Date issued
   - Expiration date
   - Attestation type
   - Issuer name

## Understanding Your Attestations

### Attestation Details

Click any attestation to view:

**Basic Information**:
- **Type**: What the attestation proves
- **Issuer**: Who issued it
- **Issued**: When it was created
- **Expires**: When it expires (if applicable)
- **Status**: Active, expired, or revoked

**Blockchain Information**:
- **Attestation UID**: Unique identifier
- **Schema**: Attestation structure definition
- **Chain**: Which blockchain (Base, Ethereum)
- **Transaction**: Link to block explorer
- **Revocable**: Can it be revoked

**Technical Details**:
- **Recipient**: Your wallet address
- **Attester Address**: Issuer's address
- **Ref UID**: Reference to related attestations
- **Data**: Encoded credential information

### Attestation Types

#### Identity Attestations

**KYC Verification**:
- Proves identity verification complete
- Shows verification level (Basic, Enhanced)
- Includes jurisdiction
- Typically expires after 12 months

**Age Verification**:
- Proves you meet minimum age
- No birthdate revealed
- Usually permanent (unless revoked)

**Geographic Attestation**:
- Proves your location/jurisdiction
- Used for compliance restrictions
- May need updating if you relocate

#### Qualification Attestations

**Accredited Investor**:
- Proves accredited investor status
- Shows accreditation type
- Expires after 12 months
- Enables access to private securities

**Professional Credentials**:
- Proves professional licenses
- Shows credential type
- Issuing authority
- May have expiration dates

#### Platform Attestations

**Platform Member**:
- Proves CapSign membership
- Account creation date
- Standing (good/suspended)

**Reputation Score**:
- Proves reputation level
- Based on platform activity
- Updates periodically

## Sharing Attestations

### Grant Access to Applications

Allow applications to verify your attestations:

1. Application requests attestation verification
2. You receive permission request
3. Review what they want to verify:
   - Which attestations
   - What information
   - How long access lasts
4. Approve or deny

**You Control**:
- Who can verify
- Which attestations they can see
- Duration of access
- Revoke access anytime

### Generate Verification Link

Create a link others can use to verify:

1. Select attestation
2. Click **Share**
3. Choose sharing method:
   - **Public Link**: Anyone with link can verify
   - **Private Link**: Requires authentication
   - **Time-Limited**: Expires after duration
4. Share link with intended party

### Verify for Others

Prove your attestations to another party:

1. Navigate to **Attestations** → **Verify**
2. Select attestations to share
3. Generate verification code or QR
4. Other party can verify using code
5. No personal data revealed

## Managing Attestations

### Renewing Expired Attestations

When attestations expire:

1. You'll receive notification before expiration
2. Navigate to expired attestation
3. Click **Renew**
4. Complete renewal process:
   - Confirm information still accurate
   - Upload new documents (if needed)
   - Pay renewal fee (if applicable)
5. New attestation issued upon approval

### Requesting New Attestations

To obtain additional attestations:

1. Go to **Attestations** → **Request**
2. Browse available attestations
3. Select desired type
4. Complete requirements:
   - Submit documents
   - Complete verification
   - Pass checks
5. Attestation issued when approved

### Understanding Revoked Attestations

If an attestation is revoked:

**Why Revocation Happens**:
- Information no longer accurate
- Failed compliance check
- Voluntary revocation
- Issuer decision

**What It Means**:
- Attestation no longer valid
- Applications can see revoked status
- May lose access to features

**What to Do**:
- Contact issuer for reason
- Obtain new attestation if eligible
- Update information if needed

## Verifying Others' Attestations

### Check Someone's Attestations

To verify another user's attestations:

1. Navigate to **Verify** → **Check Attestation**
2. Enter:
   - Wallet address, OR
   - Attestation UID, OR
   - Verification link
3. View results:
   - Attestation details
   - Issuer information
   - Current status
   - Issuance and expiration dates

### What You Can See

Public attestation information:
- Attestation exists
- Type of credential
- Issuer
- Issuance date
- Expiration date
- Current status (active/revoked)

**You CANNOT see**:
- Personal information
- Underlying documents
- Private data
- Specifics beyond attestation fact

## Attestation Use Cases

### Platform Features

Attestations enable access to features:

**Document Signing**:
- Requires KYC attestation
- May require age verification
- Specific documents may need additional attestations

**Investment Opportunities**:
- Requires accredited investor attestation
- Geographic attestations for jurisdiction compliance
- KYC for all participants

**Premium Features**:
- May require reputation attestations
- Professional credentials for certain features
- Platform membership tier attestations

### Third-Party Applications

Use CapSign attestations elsewhere:

- **DeFi Platforms**: Compliance verification
- **NFT Marketplaces**: Age verification
- **DAOs**: Membership proof
- **Other dApps**: Identity credentials

### Regulatory Compliance

Attestations prove compliance with:

- KYC/AML regulations
- Securities laws
- Age restrictions
- Geographic limitations
- Professional requirements

## Privacy and Security

### What's On-Chain vs Off-Chain

**Stored On-Chain** (Public):
- Attestation exists
- Your wallet address
- Issuer address
- Schema (attestation type)
- Timestamps
- Status (active/revoked)

**Stored Off-Chain** (Private):
- Your personal information
- ID documents
- Financial details
- Specific verification data

### Protecting Your Privacy

**Best Practices**:
- Only share attestations when necessary
- Use time-limited sharing links
- Review and revoke unused permissions
- Monitor who has access
- Be cautious with public links

### Controlling Access

You control attestation access:

1. **Settings** → **Attestation Privacy**
2. Configure:
   - Default sharing permissions
   - Who can request verification
   - Automatic approval settings
   - Notification preferences

## Troubleshooting

### Attestation Not Showing

**Solutions**:
- Refresh page
- Check you're viewing correct network (Base/Ethereum)
- Verify attestation was issued to your address
- Wait a few minutes for blockchain sync
- Contact issuer if missing

### Can't Verify Attestation

**Issues**:
- Invalid attestation UID
- Wrong network selected
- Attestation was revoked
- Network connectivity

**Solutions**:
- Double-check UID
- Switch networks (Base/Ethereum)
- Verify status with issuer
- Try different browser

### Renewal Rejected

**Reasons**:
- Information changed
- Failed updated checks
- Incomplete documentation
- Expired documents

**Solutions**:
- Review rejection reason
- Update documents
- Correct information
- Resubmit request
- Contact support if needed

### Permission Request Failed

**Solutions**:
- Ensure you have required attestations
- Check attestations haven't expired
- Verify not revoked
- Try approving again
- Contact application support

## Advanced Features

### Attestation Notifications

Configure alerts:

1. **Settings** → **Notifications** → **Attestations**
2. Enable notifications for:
   - New attestations issued
   - Expiration warnings (30, 7, 1 days)
   - Revocation notices
   - Verification requests
   - Access grants

### Attestation Analytics

View attestation insights:

1. Navigate to **Attestations** → **Analytics**
2. See:
   - How many times verified
   - Which applications accessed
   - Verification trends
   - Attestation timeline

### Exporting Attestation Data

Export your attestation information:

1. **Attestations** → **Export**
2. Choose format:
   - **CSV**: Spreadsheet format
   - **JSON**: Technical format
   - **PDF Report**: Human-readable

3. Use for:
   - Personal records
   - Compliance reporting
   - Application integrations

## Getting Help

Need attestation assistance?

- **[FAQ](/getting-started/faq.md#attestations)** - Common questions
- **[Discord](https://discord.gg/gSmnZ9wmNv)** - Community support
- **[Email](mailto:support@capsign.com)** - Direct support
- **[Protocol Docs](/protocol/attestations.md)** - Technical details

---

**Next:** [Security Best Practices](/guides/security-best-practices.md) →

