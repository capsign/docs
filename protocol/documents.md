# Document Architecture

Blockchain-based document management with cryptographic signatures.

## Overview

Features:
- Upload to IPFS/Arweave
- Cryptographic signatures
- EAS attestations for documents
- Multi-party signing
- Verification tools

## Architecture

Documents are NOT stored in diamonds - they use EAS attestations:

**Storage:**
- Document content → IPFS/Arweave (off-chain)
- Document metadata → EAS attestation (on-chain)
- Signatures → EAS attestations (on-chain)

**Wallet Integration:**
- `WalletDocumentsFacet` - Per-wallet document tracking
- Local storage of document IDs
- Links to EAS attestations

## Document Flow

### 1. Upload Document

```solidity
// Via wallet facet
bytes32 documentId = wallet.uploadDocument({
  contentHash: sha256(documentContent),
  storageURI: ipfsURI,
  category: "legal",
  requiredSigners: [signer1, signer2],
  title: "Subscription Agreement"
});

// Creates EAS attestation
```

### 2. Sign Document

```solidity
// Via wallet facet
wallet.signDocument(documentId);

// Creates signature attestation on EAS
```

### 3. Verify

```solidity
// Check signature on EAS
(bool signed, uint256 timestamp) = wallet.isDocumentSigned(signer, documentId);
```

## EAS Schemas

### Document Schema

```solidity
struct DocumentAttestation {
  bytes32 contentHash;     // SHA-256 of document
  string storageURI;       // IPFS/Arweave URI
  string category;         // Document category
  string title;            // Document title
  address[] requiredSigners; // Who must sign
  uint256 createdAt;       // Creation timestamp
}
```

### Signature Schema

```solidity
struct SignatureAttestation {
  bytes32 documentUID;     // Reference to document
  address signer;          // Who signed
  uint256 signedAt;        // When signed
  bytes signature;         // Cryptographic signature
}
```

## Wallet Documents Facet

### Interface

```solidity
interface IWalletDocuments {
  // Upload document (creates EAS attestation)
  function uploadDocument(
    bytes32 contentHash,
    string calldata storageURI,
    string calldata category,
    address[] calldata requiredSigners,
    string calldata title
  ) external returns (bytes32 easUID);
  
  // Sign document (creates signature attestation)
  function signDocument(bytes32 documentUID) external;
  
  // Check if signed
  function isDocumentSigned(address signer, bytes32 documentUID) 
    external view returns (bool signed, uint256 timestamp);
  
  // Get all documents
  function getWalletDocuments() external view returns (bytes32[] memory);
  
  // Get documents by category
  function getDocumentsByCategory(string calldata category) 
    external view returns (bytes32[] memory);
}
```

### Storage

Minimal storage in wallet:

```solidity
library WalletDocumentsStorage {
  struct Layout {
    bytes32[] allDocuments;
    mapping(string => bytes32[]) documentsByCategory;
    mapping(address => mapping(bytes32 => uint256)) signatures;
  }
}
```

Actual document data in EAS.

## EAS Integration

### Creating Document Attestation

```solidity
function uploadDocument(...) external returns (bytes32 easUID) {
  easUID = eas.attest(AttestationRequest({
    schema: DOCUMENT_SCHEMA_UID,
    data: AttestationRequestData({
      recipient: address(this), // Wallet address
      expirationTime: 0,
      revocable: true,
      refUID: 0,
      data: abi.encode(contentHash, storageURI, category, title, requiredSigners),
      value: 0
    })
  }));
  
  // Store in wallet
  WalletDocumentsStorage.layout().allDocuments.push(easUID);
  WalletDocumentsStorage.layout().documentsByCategory[category].push(easUID);
}
```

### Creating Signature Attestation

```solidity
function signDocument(bytes32 documentUID) external {
  // Create signature attestation
  bytes32 signatureUID = eas.attest(AttestationRequest({
    schema: SIGNATURE_SCHEMA_UID,
    data: AttestationRequestData({
      recipient: msg.sender,
      expirationTime: 0,
      revocable: false,
      refUID: documentUID, // Reference to document
      data: abi.encode(msg.sender, block.timestamp),
      value: 0
    })
  }));
  
  // Record signature
  WalletDocumentsStorage.layout().signatures[msg.sender][documentUID] = block.timestamp;
  
  emit DocumentSigned(documentUID, msg.sender);
}
```

## Verification

### Off-Chain Verification

1. Get document attestation from EAS
2. Download document from storage URI
3. Calculate content hash
4. Compare with attestation hash
5. Check signature attestations

### Smart Contract Verification

```solidity
function verifyDocument(bytes32 documentUID, bytes memory documentContent) 
  external view returns (bool valid) {
  
  Attestation memory docAttestation = eas.getAttestation(documentUID);
  
  // Check not revoked
  if (docAttestation.revocationTime != 0) return false;
  
  // Check content hash
  bytes32 calculatedHash = sha256(documentContent);
  (bytes32 storedHash,,,,,) = abi.decode(docAttestation.data, (bytes32, string, string, string, address[]));
  
  return calculatedHash == storedHash;
}
```

## Security

### Content Integrity

- SHA-256 hash prevents tampering
- Any change invalidates signature
- Hash stored immutably on-chain

### Signature Authenticity

- ERC-1271 wallet signatures
- Biometric authentication required
- Replay protection via nonces

### Privacy

- Only hash on-chain (not content)
- Document stored off-chain
- Access control via storage layer

## Events

```solidity
event DocumentUploaded(bytes32 indexed documentId, bytes32 indexed contentHash, address indexed creator);
event DocumentSigned(bytes32 indexed documentId, address indexed signer, uint256 timestamp);
event DocumentRevoked(bytes32 indexed documentId, address indexed revokedBy);
```

## Gas Costs

On Base:
- Create document attestation: ~100k gas (~$0.001)
- Sign document: ~80k gas (~$0.0008)
- Verify (off-chain): Free

## Testing

```solidity
function testUploadDocument() public {
  bytes32 docUID = wallet.uploadDocument({
    contentHash: keccak256("Test doc"),
    storageURI: "ipfs://...",
    category: "test",
    requiredSigners: new address[](0),
    title: "Test Document"
  });
  
  assertEq(wallet.getWalletDocuments().length, 1);
}

function testSignDocument() public {
  vm.prank(signer);
  wallet.signDocument(docUID);
  
  (bool signed,) = wallet.isDocumentSigned(signer, docUID);
  assertTrue(signed);
}
```

## Resources

- [EAS Documentation](https://docs.attest.sh)
- [IPFS Documentation](https://docs.ipfs.io)
- [Arweave Documentation](https://docs.arweave.org)
