# Compliance Framework Implementation

This guide provides detailed technical implementation details for CapSign's compliance framework, including smart contract integration, attestation management, and regulatory compliance procedures.

## 🏗️ Technical Architecture

### Smart Contract Integration

#### Attestation Registry Contract

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

import "@ethereum-attestation-service/eas-contracts/contracts/IEAS.sol";
import "@ethereum-attestation-service/eas-contracts/contracts/ISchemaRegistry.sol";

contract CapSignCompliance {
    IEAS public immutable eas;
    ISchemaRegistry public immutable schemaRegistry;

    // Schema UIDs for different attestation types
    bytes32 public constant KYC_SCHEMA_UID = 0x...;
    bytes32 public constant INVESTOR_QUALIFICATION_UID = 0x...;
    bytes32 public constant JURISDICTION_SCHEMA_UID = 0x...;

    // Compliance roles
    mapping(address => bool) public authorizedAttester;
    mapping(address => bool) public complianceOfficer;

    struct ComplianceStatus {
        bool kycVerified;
        bool qualified;
        uint256 investmentLimit;
        uint64 expirationDate;
        string jurisdiction;
    }

    mapping(address => ComplianceStatus) public userCompliance;

    constructor(address _eas, address _schemaRegistry) {
        eas = IEAS(_eas);
        schemaRegistry = ISchemaRegistry(_schemaRegistry);
    }

    function verifyCompliance(address user) external view returns (bool) {
        ComplianceStatus memory status = userCompliance[user];
        return status.kycVerified &&
               status.qualified &&
               block.timestamp < status.expirationDate;
    }

    function updateComplianceStatus(
        address user,
        bytes32 attestationUID
    ) external onlyAuthorizedAttester {
        // Verify attestation exists and is valid
        Attestation memory attestation = eas.getAttestation(attestationUID);
        require(attestation.recipient == user, "Invalid recipient");
        require(!attestation.revoked, "Attestation revoked");

        // Update compliance status based on attestation data
        _updateUserCompliance(user, attestation);
    }

    function _updateUserCompliance(
        address user,
        Attestation memory attestation
    ) internal {
        // Decode attestation data and update compliance status
        // Implementation depends on specific schema structure
    }
}
```

#### Investment Contract Integration

```solidity
contract InvestmentOffering {
    CapSignCompliance public complianceContract;

    modifier onlyCompliantUser(address investor) {
        require(
            complianceContract.verifyCompliance(investor),
            "User not compliant"
        );
        _;
    }

    function invest(uint256 amount) external onlyCompliantUser(msg.sender) {
        // Investment logic with compliance checks
        ComplianceStatus memory status = complianceContract.userCompliance(msg.sender);
        require(amount <= status.investmentLimit, "Exceeds investment limit");

        // Process investment
        _processInvestment(msg.sender, amount);
    }
}
```

### Attestation Schema Definitions

#### KYC/AML Schema

```typescript
interface KYCAttestationData {
  kycVerified: boolean;
  amlCleared: boolean;
  riskScore: number; // 0-100
  verificationDate: bigint;
  expirationDate: bigint;
  jurisdiction: string;
  verificationMethod: string;
  documentHash: string; // Hash of verification documents
}

const KYC_SCHEMA =
  "bool kyc_verified,bool aml_cleared,uint8 risk_score,uint64 verification_date,uint64 expiration_date,string jurisdiction,string verification_method,bytes32 document_hash";
```

#### Investor Qualification Schema

```typescript
interface InvestorQualificationData {
  accredited: boolean;
  investmentLimit: bigint;
  qualificationType: string; // "accredited", "sophisticated", "retail"
  verificationDate: bigint;
  expirationDate: bigint;
  jurisdictions: string[];
  incomeVerified: boolean;
  netWorthVerified: boolean;
  professionalCertification: boolean;
}

const INVESTOR_SCHEMA =
  "bool accredited,uint256 investment_limit,string qualification_type,uint64 verification_date,uint64 expiration_date,string[] jurisdictions,bool income_verified,bool net_worth_verified,bool professional_certification";
```

#### Jurisdiction Schema

```typescript
interface JurisdictionData {
  residenceJurisdictions: string[];
  taxJurisdictions: string[];
  restrictions: string[];
  investmentPermissions: boolean[];
  verificationDate: bigint;
  sanctionsCleared: boolean;
  pepStatus: boolean;
}

const JURISDICTION_SCHEMA =
  "string[] residence_jurisdictions,string[] tax_jurisdictions,string[] restrictions,bool[] investment_permissions,uint64 verification_date,bool sanctions_cleared,bool pep_status";
```

## 🔐 Verification Process Implementation

### Document Processing Pipeline

#### Document Upload Service

```typescript
import { encrypt, hash } from "./crypto-utils";

class DocumentProcessor {
  async processDocument(
    userId: string,
    documentType: string,
    fileBuffer: Buffer
  ): Promise<DocumentProcessingResult> {
    // 1. Validate document format and size
    const validation = await this.validateDocument(fileBuffer, documentType);
    if (!validation.valid) {
      throw new Error(`Invalid document: ${validation.reason}`);
    }

    // 2. Generate document hash for integrity
    const documentHash = hash(fileBuffer);

    // 3. Encrypt document for secure storage
    const encryptedDocument = await encrypt(fileBuffer);

    // 4. Store encrypted document
    const storageId = await this.storeDocument(encryptedDocument);

    // 5. Create verification task
    const taskId = await this.createVerificationTask({
      userId,
      documentType,
      documentHash,
      storageId,
    });

    return {
      taskId,
      documentHash,
      status: "pending_verification",
    };
  }

  private async validateDocument(
    buffer: Buffer,
    type: string
  ): Promise<ValidationResult> {
    // Implement document validation logic
    // - File format validation
    // - Size limits
    // - Content validation
    // - Security scanning
  }
}
```

#### Automated Verification Service

```typescript
class AutomatedVerification {
  async processVerificationTask(taskId: string): Promise<VerificationResult> {
    const task = await this.getTask(taskId);
    const document = await this.retrieveDocument(task.storageId);

    // 1. OCR and text extraction
    const extractedData = await this.extractDocumentData(document);

    // 2. Document authenticity verification
    const authenticityCheck = await this.verifyDocumentAuthenticity(
      extractedData
    );

    // 3. Data validation
    const dataValidation = await this.validateExtractedData(extractedData);

    // 4. Risk assessment
    const riskScore = await this.calculateRiskScore(extractedData);

    // 5. Sanctions and PEP screening
    const screeningResult = await this.performScreening(extractedData);

    const result: VerificationResult = {
      taskId,
      automated: true,
      passed: authenticityCheck.valid && dataValidation.valid,
      riskScore,
      screeningResult,
      requiresHumanReview: riskScore > 70 || !authenticityCheck.confident,
      extractedData,
    };

    if (!result.requiresHumanReview) {
      await this.finalizeVerification(result);
    }

    return result;
  }
}
```

#### Human Review Service

```typescript
class HumanReviewService {
  async submitForReview(verificationResult: VerificationResult): Promise<void> {
    const reviewTask = {
      taskId: verificationResult.taskId,
      priority: this.calculatePriority(verificationResult),
      assignedTo: await this.assignReviewer(verificationResult),
      submittedAt: new Date(),
      documents: await this.prepareReviewPackage(verificationResult),
    };

    await this.reviewQueue.add(reviewTask);
    await this.notifyReviewer(reviewTask);
  }

  async completeReview(
    taskId: string,
    reviewerId: string,
    decision: ReviewDecision
  ): Promise<void> {
    const task = await this.getReviewTask(taskId);

    // Validate reviewer authorization
    await this.validateReviewer(reviewerId, task);

    // Record review decision
    const reviewRecord = {
      taskId,
      reviewerId,
      decision,
      timestamp: new Date(),
      comments: decision.comments,
      secondReviewRequired: decision.requiresSecondReview,
    };

    await this.recordReview(reviewRecord);

    if (decision.approved && !decision.requiresSecondReview) {
      await this.finalizeVerification(task);
    } else if (decision.requiresSecondReview) {
      await this.requestSecondReview(task);
    } else {
      await this.rejectVerification(task, decision.reason);
    }
  }
}
```

### Attestation Creation Service

```typescript
class AttestationService {
  constructor(
    private eas: EAS,
    private schemaRegistry: SchemaRegistry,
    private privateKey: string
  ) {}

  async createKYCAttestation(
    userId: string,
    verificationResult: VerificationResult
  ): Promise<string> {
    const attestationData: KYCAttestationData = {
      kycVerified: verificationResult.passed,
      amlCleared: verificationResult.screeningResult.passed,
      riskScore: verificationResult.riskScore,
      verificationDate: BigInt(Date.now()),
      expirationDate: BigInt(Date.now() + 365 * 24 * 60 * 60 * 1000), // 1 year
      jurisdiction: verificationResult.extractedData.jurisdiction,
      verificationMethod: verificationResult.method,
      documentHash: verificationResult.documentHash,
    };

    const encodedData = this.encodeAttestationData(attestationData, KYC_SCHEMA);

    const attestation = await this.eas.attest({
      schema: KYC_SCHEMA_UID,
      data: {
        recipient: userId,
        expirationTime: attestationData.expirationDate,
        revocable: true,
        data: encodedData,
      },
    });

    return attestation.uid;
  }

  async createInvestorQualificationAttestation(
    userId: string,
    qualificationData: InvestorQualificationData
  ): Promise<string> {
    const encodedData = this.encodeAttestationData(
      qualificationData,
      INVESTOR_SCHEMA
    );

    const attestation = await this.eas.attest({
      schema: INVESTOR_QUALIFICATION_UID,
      data: {
        recipient: userId,
        expirationTime: qualificationData.expirationDate,
        revocable: true,
        data: encodedData,
      },
    });

    return attestation.uid;
  }

  private encodeAttestationData(data: any, schema: string): string {
    // Implement proper ABI encoding for attestation data
    return ethers.utils.defaultAbiCoder.encode(
      this.parseSchema(schema),
      Object.values(data)
    );
  }
}
```

## 📊 Compliance Monitoring System

### Real-time Compliance Checker

```typescript
class ComplianceMonitor {
  async checkUserCompliance(userId: string): Promise<ComplianceStatus> {
    const attestations = await this.getUserAttestations(userId);

    const kycAttestation = attestations.find(
      (a) => a.schema === KYC_SCHEMA_UID
    );
    const qualificationAttestation = attestations.find(
      (a) => a.schema === INVESTOR_QUALIFICATION_UID
    );
    const jurisdictionAttestation = attestations.find(
      (a) => a.schema === JURISDICTION_SCHEMA_UID
    );

    const status: ComplianceStatus = {
      kycVerified: this.isAttestationValid(kycAttestation),
      qualified: this.isAttestationValid(qualificationAttestation),
      jurisdictionVerified: this.isAttestationValid(jurisdictionAttestation),
      investmentLimit: await this.calculateInvestmentLimit(
        qualificationAttestation
      ),
      expirationDate: this.getEarliestExpiration(attestations),
      restrictions: await this.getApplicableRestrictions(
        jurisdictionAttestation
      ),
    };

    return status;
  }

  async monitorExpirations(): Promise<void> {
    const expiringAttestations = await this.getExpiringAttestations(30); // 30 days

    for (const attestation of expiringAttestations) {
      await this.sendExpirationNotification(attestation);
      await this.createRenewalTask(attestation);
    }
  }

  async validateInvestment(
    userId: string,
    offeringId: string,
    amount: bigint
  ): Promise<InvestmentValidationResult> {
    const compliance = await this.checkUserCompliance(userId);
    const offering = await this.getOffering(offeringId);

    const validation: InvestmentValidationResult = {
      allowed: true,
      reasons: [],
    };

    // Check KYC status
    if (!compliance.kycVerified) {
      validation.allowed = false;
      validation.reasons.push("KYC verification required");
    }

    // Check investor qualification
    if (offering.requiresAccreditation && !compliance.qualified) {
      validation.allowed = false;
      validation.reasons.push("Accredited investor status required");
    }

    // Check investment limits
    if (amount > compliance.investmentLimit) {
      validation.allowed = false;
      validation.reasons.push(
        `Investment exceeds limit of ${compliance.investmentLimit}`
      );
    }

    // Check jurisdictional restrictions
    const jurisdictionCheck = await this.checkJurisdictionalCompliance(
      userId,
      offering.restrictedJurisdictions
    );

    if (!jurisdictionCheck.allowed) {
      validation.allowed = false;
      validation.reasons.push(...jurisdictionCheck.reasons);
    }

    return validation;
  }
}
```

### Regulatory Reporting Service

```typescript
class RegulatoryReporting {
  async generateComplianceReport(
    startDate: Date,
    endDate: Date,
    jurisdiction: string
  ): Promise<ComplianceReport> {
    const activities = await this.getComplianceActivities(startDate, endDate);
    const attestations = await this.getAttestationsInPeriod(startDate, endDate);
    const violations = await this.getComplianceViolations(startDate, endDate);

    const report: ComplianceReport = {
      reportPeriod: { startDate, endDate },
      jurisdiction,
      summary: {
        totalUsers: await this.getUserCount(),
        verifiedUsers: await this.getVerifiedUserCount(),
        newVerifications: activities.verifications.length,
        expiredVerifications: activities.expirations.length,
        violations: violations.length,
      },
      activities,
      attestations: attestations.map(this.sanitizeAttestation),
      violations,
      generatedAt: new Date(),
      reportHash: "", // Will be computed after serialization
    };

    report.reportHash = this.computeReportHash(report);

    return report;
  }

  async submitRegulatoryFiling(
    report: ComplianceReport,
    regulator: string
  ): Promise<void> {
    const submission = {
      reportId: report.reportHash,
      regulator,
      submissionDate: new Date(),
      reportData: this.formatForRegulator(report, regulator),
    };

    await this.fileWithRegulator(submission);
    await this.recordSubmission(submission);
  }
}
```

## 🔧 Configuration and Deployment

### Environment Configuration

```typescript
interface ComplianceConfig {
  // Ethereum Attestation Service configuration
  easContractAddress: string;
  schemaRegistryAddress: string;
  schemaUIDs: {
    kyc: string;
    investorQualification: string;
    jurisdiction: string;
  };

  // Verification settings
  verificationSettings: {
    requireHumanReview: boolean;
    riskThreshold: number;
    expirationPeriod: number;
    autoRenewalEnabled: boolean;
  };

  // Integration endpoints
  apis: {
    sanctionsScreening: string;
    documentVerification: string;
    riskAssessment: string;
  };

  // Regulatory compliance
  enabledJurisdictions: string[];
  regulatoryReporting: {
    enabled: boolean;
    frequency: string; // 'daily', 'weekly', 'monthly'
    recipients: string[];
  };
}
```

### Deployment Scripts

```typescript
async function deployComplianceFramework(config: ComplianceConfig) {
  // 1. Register attestation schemas
  const schemaRegistry = new SchemaRegistry(config.schemaRegistryAddress);

  const kycSchemaUID = await schemaRegistry.register({
    schema: KYC_SCHEMA,
    resolver: ethers.constants.AddressZero,
    revocable: true,
  });

  const investorSchemaUID = await schemaRegistry.register({
    schema: INVESTOR_SCHEMA,
    resolver: ethers.constants.AddressZero,
    revocable: true,
  });

  const jurisdictionSchemaUID = await schemaRegistry.register({
    schema: JURISDICTION_SCHEMA,
    resolver: ethers.constants.AddressZero,
    revocable: true,
  });

  // 2. Deploy compliance contract
  const complianceContract = await deployContract("CapSignCompliance", [
    config.easContractAddress,
    config.schemaRegistryAddress,
  ]);

  // 3. Configure services
  await configureVerificationService(config);
  await configureMonitoringService(config);
  await configureReportingService(config);

  console.log("Compliance framework deployed successfully!");
  console.log(`Contract address: ${complianceContract.address}`);
  console.log(`KYC Schema UID: ${kycSchemaUID}`);
  console.log(`Investor Schema UID: ${investorSchemaUID}`);
  console.log(`Jurisdiction Schema UID: ${jurisdictionSchemaUID}`);
}
```

---

**Ready to implement?** This comprehensive implementation guide provides the technical foundation for deploying CapSign's compliance framework. For additional support, contact our technical team at [developers@capsign.com](mailto:developers@capsign.com).
