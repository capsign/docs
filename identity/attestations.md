# Attestations

Learn about attestations - the cryptographic credentials that power CapSign's identity system.

## What are Attestations?

Attestations are **cryptographic credentials** that prove something about you without revealing personal information.

**Example:**
- ✅ "This wallet completed KYC on 2024-01-15"
- ❌ "This wallet belongs to John Doe, DOB 1990-01-01, SSN..."

## Ethereum Attestation Service (EAS)

CapSign uses **EAS** for all attestations:

- **On-chain** - Stored on Base blockchain
- **Privacy-preserving** - No personal data on-chain
- **Verifiable** - Anyone can verify authenticity
- **Revocable** - Can be revoked if needed
- **Portable** - Use across multiple platforms

## Types of Attestations

### KYC Attestation

Proves identity verified.

**Schema:**
- Wallet address
- Verification level
- Verification date
- Issuer (CapSign)

**Issued by:** CapSign (via Persona/Bridge.xyz)

### Accreditation Attestation

Proves accredited investor status.

**Schema:**
- Wallet address
- Qualification type
- Verification date
- Expiration date
- Issuer address

**Issued by:** Issuers (fund managers, companies)

### Custom Attestations

Other credentials:
- Qualified Purchaser
- Professional Investor
- Employment verification
- Jurisdiction confirmation

## How Attestations Work

### 1. Issuance

Issuer creates attestation:

```
Issuer signs:
  - Recipient address
  - Schema UID
  - Attestation data
  - Expiration (optional)

→ Attestation stored on-chain
```

### 2. Verification

Smart contracts check attestations:

```solidity
function checkKYC(address user) returns (bool) {
  return eas.getAttestation(user, KYC_SCHEMA).isValid();
}
```

### 3. Revocation

Issuers can revoke if needed:

```
Issuer revokes attestation
→ Status changes to "Revoked"
→ Smart contracts reject revoked attestations
```

## Viewing Your Attestations

### In CapSign

1. Navigate to **Profile** → **Attestations**
2. See all attestations linked to your wallet
3. View details:
   - Type
   - Issuer
   - Issue date
   - Expiration
   - Status

### On EAS Explorer

1. Visit [base.easscan.org](https://base.easscan.org)
2. Enter your wallet address
3. See all attestations
4. View full on-chain data

## Privacy

### What's Public

- Attestation type (KYC, Accreditation, etc.)
- Issuer address
- Issue date
- Expiration date
- Status (valid/revoked)

### What's Private

- Your name
- Your address
- Your ID documents
- Your financial information

**Attestations prove you're verified without revealing why or how.**

## Use Cases

### Investment Offerings

- Prove KYC without sharing personal info
- Prove accreditation without revealing net worth
- Satisfy compliance requirements automatically

### Document Signing

- Prove identity to sign contracts
- Demonstrate authority to act on behalf of entity
- Create verifiable signatures

### Access Control

- Grant platform access
- Unlock features
- Demonstrate qualifications

## Attestation Lifecycle

### 1. Request

Investor needs attestation for specific purpose.

### 2. Verification

Issuer verifies qualification:
- KYC: Via Persona/Bridge.xyz
- Accreditation: Review investor documentation

### 3. Issuance

Issuer signs and submits attestation on-chain.

### 4. Active

Attestation valid and usable.

### 5. Expiration or Revocation

Attestation no longer valid:
- Expires after set time
- Or revoked by issuer

## Best Practices

### For Holders

- **Keep current** - Renew before expiration
- **Verify issuers** - Only trust reputable issuers
- **Monitor status** - Check attestations haven't been revoked
- **Protect wallet** - Attestations tied to your wallet

### For Issuers

- **Verify thoroughly** - Check documentation carefully
- **Set expirations** - Require periodic re-verification
- **Document process** - Keep verification records
- **Revoke when needed** - If circumstances change

## Technical Details

### Attestation Structure

```solidity
struct Attestation {
  bytes32 uid;           // Unique ID
  bytes32 schema;        // Schema UID
  uint64 time;           // Timestamp
  uint64 expirationTime; // Expiration (0 = no expiration)
  uint64 revocationTime; // Revocation time (0 = not revoked)
  bytes32 refUID;        // Reference UID
  address recipient;     // Recipient address
  address attester;      // Issuer address
  bool revocable;        // Can be revoked?
  bytes data;            // Attestation data
}
```

### Schemas

Each attestation type has a schema:

**KYC Schema:**
```
string verificationType
string verificationLevel
uint256 verificationDate
string provider
```

**Accreditation Schema:**
```
string qualificationType
uint256 verificationDate
uint256 expirationDate
string supportingDocHash
```

## FAQs

**Q: Can I have multiple attestations?**
A: Yes, you can have many attestations from different issuers.

**Q: Are attestations transferable?**
A: No, they're tied to your wallet address.

**Q: What if my attestation is revoked?**
A: Contact the issuer to understand why and how to get a new one.

**Q: Can I see who verified me?**
A: Yes, the issuer address is public.

**Q: How long do attestations last?**
A: Depends on attestation. Some expire, others permanent until revoked.

## See Also

- [Persona Verification](persona-verification.md)
- [Accreditation](accreditation.md)
- [EAS Documentation](https://docs.attest.sh)
