# Identity

CapSign uses decentralized identity and attestations built on Ethereum Attestation Service (EAS).

## What is Identity on CapSign?

Identity on CapSign consists of:

- **KYC Verification** - Identity verification through Persona (via Bridge.xyz)
- **Attestations** - Privacy-preserving credentials on Ethereum Attestation Service
- **Accreditation** - Issuer-verified attestations for investment qualifications

## Key Concepts

### Attestations

An **attestation** is a cryptographic credential that proves something about you:

- Stored on-chain via Ethereum Attestation Service
- Privacy-preserving (no personal data on-chain)
- Portable (use across platforms)
- Revocable (if needed)

### KYC vs. Attestations

**KYC** (via Persona):
- Full identity verification
- Personal information collected
- Stored off-chain
- Results in attestation

**Attestations** (on EAS):
- Cryptographic proof only
- No personal data
- Stored on-chain
- Verifiable by smart contracts

## Types of Attestations

### 1. KYC Attestation

Proves you've completed identity verification.

**Contains:**
- Wallet address
- Verification status
- Verification date
- Issuer (CapSign via Bridge.xyz/Persona)

**Required for:**
- Investing in offerings
- Creating offerings (entity KYC)
- Certain platform features

Learn more: [Persona Verification](persona-verification.md)

### 2. Accreditation Attestation

Proves you're an accredited investor.

**Issued by:**
- Issuers (after reviewing documentation)
- Verification services
- Self-attestation (in some jurisdictions)

**Required for:**
- Reg D 506(c) offerings
- Other accredited-only investments

Learn more: [Accreditation](accreditation.md)

### 3. Custom Attestations

Other credentials issuers may require:

- Qualified Purchaser status
- Professional investor credentials
- Jurisdiction verification
- Employment verification

## For Investors

### Getting Started

1. **Create Account** - 30 seconds, no email required
2. **Explore Platform** - Browse offerings, learn about features
3. **Complete KYC** - When ready to invest
4. **Get Attestations** - As needed for specific investments

### KYC Process

1. Navigate to **Settings** → **Verification**
2. Complete Persona verification (via Bridge.xyz)
3. Provide ID and basic information
4. Receive KYC attestation (1-5 minutes)

### Accreditation

If investing in accredited-only offerings:

1. Contact issuer with documentation
2. Issuer reviews and verifies
3. Issuer issues accreditation attestation via CapSign UI
4. Attestation available immediately

## For Issuers

### Verifying Investors

Check investor attestations:

- View investor profile
- See all attestations
- Verify on EAS
- Check expiration and revocation status

### Issuing Attestations

Issue accreditation attestations:

1. Navigate to admin panel
2. Select investor
3. Choose attestation type (Accredited Investor, etc.)
4. Sign and issue on-chain
5. Investor receives immediately

Learn more: [Accreditation](accreditation.md)

## Privacy

### What's On-Chain

Only attestation metadata:
- Wallet address
- Attestation type
- Issuer
- Timestamp
- Expiration (if any)

### What's Off-Chain

Personal information:
- Name
- Address
- ID documents
- Financial information

**Personal data never goes on-chain.**

## Technical Details

### Ethereum Attestation Service

CapSign uses EAS for all attestations:

- **Schema-based** - Each attestation type has a schema
- **On-chain** - Recorded on Base blockchain
- **Verifiable** - Anyone can verify
- **Revocable** - Can be revoked if needed

### Attestation Schemas

**KYC Schema:**
- Wallet address
- Verification level
- Provider (Persona/Bridge.xyz)
- Timestamp

**Accreditation Schema:**
- Wallet address
- Qualification type
- Issuer address
- Expiration date
- Supporting documentation hash

### Verification in Smart Contracts

Offerings check attestations automatically:

```solidity
function checkKYC(address investor) internal view returns (bool) {
  return eas.getAttestation(investor, KYC_SCHEMA_UID).isValid;
}
```

## Best Practices

### For Investors

- **Complete KYC early** - Don't wait until you want to invest
- **Keep attestations current** - Renew before expiration
- **Verify issuer** - Only accept attestations from trusted issuers
- **Protect privacy** - Share attestations only when needed

### For Issuers

- **Verify thoroughly** - Check documentation carefully before issuing
- **Document everything** - Keep records of verification
- **Set expirations** - Require periodic re-verification
- **Revoke if needed** - Revoke attestations if circumstances change

## FAQs

**Q: Do I need KYC to create an account?**
A: No. KYC only required for investing.

**Q: How long does KYC take?**
A: 1-5 minutes for automated verification. Up to 24 hours if manual review needed.

**Q: Can I invest without KYC?**
A: No. All investments require KYC verification.

**Q: Who can see my attestations?**
A: Attestations are public on-chain, but contain no personal information. Only attestation metadata is visible.

**Q: Can attestations be revoked?**
A: Yes. Issuers can revoke attestations if needed.

**Q: Are attestations transferable?**
A: No. Attestations are tied to your wallet address.

## Guides

- [Persona Verification](persona-verification.md) - Complete KYC verification
- [Attestations](attestations.md) - Understanding attestations
- [Accreditation](accreditation.md) - Issuer-issued attestations

## Need Help?

- **Email:** support@capsign.com
- **Twitter:** [@CapSignInc](https://twitter.com/CapSignInc)
