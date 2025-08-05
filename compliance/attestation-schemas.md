# Attestation Schemas

CapSign uses a **privacy-first attestation framework** built on the Ethereum Attestation Service (EAS) to provide comprehensive compliance verification while protecting user privacy. Our schemas follow a **public/private hybrid model** that minimizes on-chain data exposure.

## Schema Architecture

### Privacy-First Design Principles

1. **Minimal Disclosure** - Only essential compliance status on-chain
2. **Selective Access** - Detailed data for authorized parties only
3. **User Control** - Comprehensive data sharing permissions
4. **Regulatory Compliance** - GDPR/CCPA aligned from day one

### Schema Categories

Our attestation framework uses three categories of schemas:

- **Public Schemas** - Minimal compliance status (safe for public visibility)
- **Private Reference Schemas** - On-chain pointers to encrypted off-chain data
- **Valuation Schemas** - Asset valuation and pricing data with privacy controls

## Public Schemas (Minimal Disclosure)

These schemas contain only essential compliance status without exposing sensitive personal or financial data.

### Basic Compliance Status

**Purpose**: Minimal public compliance verification without sensitive data exposure

**Fields**:

```typescript
{
  isCompliant: boolean; // Overall compliance status
  complianceLevel: number; // 0=basic, 1=enhanced, 2=institutional
  jurisdictionCode: string; // Two-letter country code (US, EU, UK, etc.)
  sanctionsStatus: boolean; // Sanctions screening passed
  verificationHash: string; // Hash of underlying verification data
  validUntil: number; // Expiration timestamp
}
```

**Usage**: Basic compliance checks for offering participation without revealing personal details.

**Expiration**: 1 year (31,536,000 seconds)

### Basic Valuation Status

**Purpose**: Minimal public valuation status without sensitive pricing data

**Fields**:

```typescript
{
  isValid: boolean; // Valuation is current and valid
  valuationType: number; // 0=409A, 1=Transaction, 2=Financial
  validUntil: number; // Valuation expiration timestamp
  commitmentHash: string; // Cryptographic commitment to valuation data
}
```

**Usage**: Public verification that valid asset valuation exists without exposing pricing details.

**Expiration**: 1 year

### Basic Business Information

**Purpose**: Minimal public business data for industry sanctions and compliance checks

**Fields**:

```typescript
{
  naicsCode: number; // NAICS industry classification code
  businessType: number; // 0=individual, 1=corporation, 2=partnership, 3=trust
  incorporationCountry: string; // Two-letter country code of incorporation
  validUntil: number; // Expiration timestamp
}
```

**Usage**: Industry and entity type verification for sanctions screening and business rules.

**Expiration**: 1 year

## Private Reference Schemas (On-Chain Pointers)

These schemas point to encrypted off-chain data without exposing content, providing access controls and data integrity verification.

### Private Compliance Reference

**Purpose**: Pointer to encrypted detailed compliance data with access controls

**Fields**:

```typescript
{
  dataCommitment: string; // Cryptographic commitment to private data
  encryptedDataURI: string; // IPFS hash of encrypted detailed data
  accessControlHash: string; // Hash of access control list
  schemaVersion: number; // Private schema version for compatibility
}
```

**Usage**: References comprehensive KYC/AML data stored encrypted off-chain with selective access.

### Jurisdiction-Specific Private References

We maintain separate private reference schemas for different regulatory jurisdictions:

#### US Private Compliance Reference

- **Jurisdiction**: United States
- **Contains**: US accreditation data, income verification, professional qualifications
- **Compliance**: SEC regulations, state blue sky laws

#### EU Private Compliance Reference

- **Jurisdiction**: European Union
- **Contains**: MiFID II professional investor data, portfolio information
- **Compliance**: GDPR, MiFID II, ESMA guidelines

#### UK Private Compliance Reference

- **Jurisdiction**: United Kingdom
- **Contains**: FCA sophisticated investor classification data
- **Compliance**: UK FCA regulations, sophisticated investor rules

### Private Valuation Reference

**Purpose**: Pointer to encrypted detailed valuation data with access controls

**Fields**:

```typescript
{
  priceCommitment: string; // Cryptographic commitment to valuation range
  encryptedRangeURI: string; // IPFS hash of encrypted valuation details
  accessControlHash: string; // Access control for detailed valuation
  schemaVersion: number; // Valuation schema version
}
```

**Usage**: References detailed 409A valuations, transaction pricing, and financial analysis.

## Encrypted Off-Chain Data Structures

### Private Identity Details

Comprehensive identity verification data stored encrypted off-chain:

```typescript
interface PrivateReferenceAttestation {
  // Personal Identity Information (PII)
  fullLegalName: string;
  dateOfBirth: string;
  placeOfBirth: string;
  nationality: string;

  // Address Information
  residentialAddress: {
    street: string;
    city: string;
    state: string;
    postalCode: string;
    country: string;
  };

  // Government Identity Documents
  primaryIdentityDocument: {
    type: string; // "passport", "national_id", "drivers_license"
    number: string;
    issuingAuthority: string;
    issueDate: string;
    expiryDate: string;
    documentHash: string; // Hash of document image
  };

  // Secondary verification documents
  addressVerificationDocument?: {
    type: string; // "utility_bill", "bank_statement", "lease_agreement"
    issueDate: string;
    documentHash: string;
  };

  // Biometric verification (if performed)
  biometricVerification?: {
    faceMatch: boolean;
    livenessCheck: boolean;
    fingerprintMatch?: boolean;
    verificationScore: number; // 0-100
  };

  // Verification process details
  verificationProcess: {
    verifierName: string;
    verifierLicense: string;
    verificationMethod: string; // "manual_review", "automated_kyc", "video_call"
    verificationDate: string;
    verificationLocation: string;
    documentsVerified: string[];
    additionalChecks: string[]; // "sanctions_screening", "pep_check", "adverse_media"
  };

  // Compliance screening results
  complianceScreening: {
    sanctionsListsChecked: string[];
    pepStatus: boolean;
    adverseMediaFound: boolean;
    riskScore: number; // 0-100
    screeningProvider: string;
    screeningDate: string;
  };

  // Internal notes (sensitive)
  internalNotes?: string;
  complianceOfficerNotes?: string;

  // Metadata
  attestationId: string;
  createdTimestamp: string;
  expiryTimestamp?: string;
}
```

### Jurisdiction-Specific Qualification Data

#### US Private Qualification Data

```typescript
interface USPrivateQualificationData {
  // Accreditation Details
  accreditationStatus: number; // 0=non-accredited, 1=accredited, 2=QIB, 3=institutional
  qualificationBasis: string[]; // How they qualify (income, net worth, professional)
  professionalCertifications: string[];
  entityType?: string;
  investmentLimits?: number;

  // Financial Thresholds (encrypted)
  individualIncome?: number;
  jointIncome?: number;
  netWorth?: number;
  liquidNetWorth?: number;

  // Professional Status
  isKnowledgeableEmployee?: boolean;
  isFamilyClient?: boolean;

  // QIB-Specific
  qibStatus?: boolean;
  assetsUnderManagement?: number;
}
```

#### EU Private Qualification Data

```typescript
interface EUPrivateQualificationData {
  // MiFID II Classification
  clientCategory: number; // 0=retail, 1=professional, 2=eligible_counterparty
  isProfessionalInvestor: boolean;
  isEligibleCounterparty: boolean;

  // Qualification Criteria
  portfolioSize?: number;
  transactionFrequency?: number;
  professionalExperience?: number;

  // Suitability Assessment
  knowledgeExperience: number; // Score 0-10
  financialSituation: string;
  investmentObjectives: string[];
  riskTolerance: number; // 0-10
  appropriatenessAssessment: boolean;
  suitabilityAssessment: boolean;
}
```

### Private Valuation Details

```typescript
interface PrivateValuationDetails {
  // Core Valuation (encrypted)
  valuationAmount: number;
  pricePerShare: number;
  sharesOutstanding: number;
  currency: string;

  // Methodology
  valuationMethod: number; // 0=DCF, 1=Market Multiple, etc.
  discountRate: number; // Basis points
  revenueMultiple?: number;
  ebitdaMultiple?: number;

  // Adjustments
  liquidityDiscount?: number;
  controlPremium?: number;
  marketabilityDiscount?: number;

  // Professional Details
  valuationFirm: string;
  leadValuer: string;
  valuerCredentials: string[];
  independentReview: boolean;
  boardApproval: boolean;
  auditCommitteeReview: boolean;

  // Quality Metrics
  confidenceLevel: number; // 1-10
  methodologyNotes: string;
  supportingDocuments: string[];
}
```

## Privacy Controls

### User Data Management

Users have comprehensive control over their attestation data:

- **Authorize Viewers**: Grant specific parties access to private data
- **Revoke Access**: Remove access permissions at any time
- **Data Expiration**: Set automatic expiration dates
- **Export Data**: Download complete data export (GDPR compliance)
- **Delete Data**: Remove private data while maintaining compliance history
- **Access Audit**: View complete log of who accessed their data

### GDPR/CCPA Compliance

- **Right to Access**: Complete data export capability
- **Right to Rectification**: Update attestation data
- **Right to Erasure**: Delete private data (subject to legal obligations)
- **Data Portability**: Export in machine-readable format
- **Consent Management**: Granular consent for each data sharing scenario

## Schema Integration

### Creating Attestations

1. **Data Separation**: Split sensitive and non-sensitive data
2. **Encryption**: Encrypt private data with user-controlled keys
3. **Off-Chain Storage**: Store encrypted data on IPFS
4. **Public Attestation**: Create minimal disclosure on-chain attestation
5. **Private Reference**: Create on-chain pointer to encrypted data
6. **Access Control**: Distribute decryption keys to authorized parties

### Verifying Compliance

1. **Public Check**: Verify basic compliance status (always accessible)
2. **Authorization Check**: Confirm viewer has access to detailed data
3. **Data Retrieval**: Fetch and decrypt private data if authorized
4. **Compliance Evaluation**: Assess detailed compliance requirements
5. **Audit Logging**: Record all data access for compliance monitoring

## Schema Evolution

Schemas support versioning to enable:

- **Backward Compatibility**: Old attestations remain valid
- **Regulatory Updates**: New requirements without breaking existing data
- **Enhanced Privacy**: Upgrade to zero-knowledge proofs when ready
- **Cross-Jurisdiction**: Support new regulatory frameworks

---

**Ready to implement attestations?** See our [Verification Process Guide](verification-process.md) for detailed document requirements and implementation steps.
