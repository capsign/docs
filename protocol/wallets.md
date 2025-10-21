# Wallet Architecture

CapSign wallets are ERC-4337 smart contract wallets built on the Diamond Pattern.

## Overview

Wallets support:
- Passkey authentication (WebAuthn/P256)
- EOA signers (MetaMask, etc.)
- ERC-4337 account abstraction
- Multi-ownership
- Document signing (ERC-1271)
- Attestation management

## Diamond Structure

```
WalletDiamond
├── DiamondCutFacet - Upgrades
├── DiamondLoupeFacet - Introspection
├── AccessControlFacet - Permissions
├── WalletCoreFacet - ERC-4337 logic
├── WalletSignatureFacet - ERC-1271 signatures
├── WalletDocumentsFacet - Document management
└── WalletIdentityFacet - Attestation management
```

## Key Facets

### WalletCoreFacet

ERC-4337 account abstraction:

```solidity
interface IWalletCore {
  // Initialize wallet
  function WalletCore_init(
    bytes[] calldata owners,
    string calldata walletType,
    uint256 requiredApprovals
  ) external;
  
  // Execute transaction
  function execute(
    address target,
    uint256 value,
    bytes calldata data
  ) external returns (bool success, bytes memory result);
  
  // ERC-4337 validation
  function validateUserOp(
    UserOperation calldata userOp,
    bytes32 userOpHash,
    uint256 missingAccountFunds
  ) external returns (uint256 validationData);
}
```

### WalletSignatureFacet

ERC-1271 signature validation:

```solidity
interface IWalletSignature {
  // Validate signature
  function isValidSignature(
    bytes32 hash,
    bytes calldata signature
  ) external view returns (bytes4 magicValue);
}
```

Supports:
- Passkey signatures (WebAuthn P-256)
- EOA signatures (ECDSA)
- Multi-sig

### WalletDocumentsFacet

Document management:

```solidity
interface IWalletDocuments {
  // Upload document
  function uploadDocument(
    bytes32 contentHash,
    string calldata storageURI,
    string calldata category,
    address[] calldata requiredSigners,
    string calldata title
  ) external returns (bytes32 documentId);
  
  // Sign document
  function signDocument(bytes32 documentId) external;
  
  // Get documents
  function getWalletDocuments() external view returns (bytes32[] memory);
}
```

### WalletIdentityFacet

Attestation management:

```solidity
interface IWalletIdentity {
  // Link attestation
  function linkAttestation(bytes32 attestationUID) external;
  
  // Get attestations
  function getAttestations() external view returns (bytes32[] memory);
}
```

## Deployment

### Via WalletFactory

```solidity
IWallet wallet = IWalletFactory(WALLET_FACTORY).createWallet({
  owners: [abi.encode(x, y)], // Passkey coordinates
  walletType: "individual",
  requiredApprovals: 1
});
```

### CREATE2 Deterministic

Wallets deployed with CREATE2 for:
- Predictable addresses
- Vanity addresses
- Cross-chain consistency

Address = `f(factory, salt, initCode)`

## Ownership

### Individual Wallets

**Passkey owners:**
```solidity
bytes memory owner = abi.encode(x, y); // P-256 public key coordinates
```

### Entity Wallets

**EOA owners:**
```solidity
bytes memory owner = abi.encode(address); // Ethereum address
```

### Multi-Ownership

Wallets can have multiple owners:
- Multiple passkeys
- Multiple EOAs
- Mix of passkeys and EOAs

## Signatures

### Passkey Signatures

WebAuthn (P-256) signatures:

```solidity
struct PasskeySignature {
  bytes authenticatorData;
  bytes clientDataJSON;
  uint256 challengeIndex;
  uint256 typeIndex;
  uint256 r;
  uint256 s;
}
```

Verified using P-256 elliptic curve.

### EOA Signatures

Standard ECDSA signatures:

```solidity
bytes memory signature = abi.encodePacked(r, s, v);
```

## Storage

### WalletCoreStorage

```solidity
library WalletCoreStorage {
  bytes32 constant STORAGE_SLOT = keccak256("wallet.core.storage");
  
  struct Layout {
    string walletType;
    uint256 requiredApprovals;
    bool emergencyMode;
    address entryPoint;
  }
}
```

### Owner Management

Owners stored using Coinbase Smart Wallet's `MultiOwnable` pattern:

- Owners indexed by position
- Can add/remove owners
- Removed owners tracked separately

## ERC-4337 Integration

### UserOperation Flow

```
1. User signs UserOp
2. Bundler calls validateUserOp()
3. Wallet validates signature
4. EntryPoint executes operation
5. Wallet executes target call
```

### Gas Sponsorship

Paymaster can sponsor gas:

```solidity
userOp.paymasterAndData = abi.encodePacked(
  paymasterAddress,
  validUntil,
  validAfter,
  signature
);
```

## Security

### Access Control

All state-changing functions protected:

```solidity
modifier onlyOwner() {
  require(isOwner(msg.sender), "Not owner");
  _;
}

modifier onlyEntryPoint() {
  require(msg.sender == entryPoint, "Not entry point");
  _;
}
```

### Reentrancy Protection

All external calls protected:

```solidity
modifier nonReentrant() {
  require(!_locked, "Reentrant call");
  _locked = true;
  _;
  _locked = false;
}
```

### Emergency Mode

Pause wallet in emergencies:

```solidity
function activateEmergencyMode() external onlyOwner {
  WalletCoreStorage.layout().emergencyMode = true;
  emit EmergencyModeActivated();
}
```

## Events

```solidity
event WalletInitialized(address indexed wallet, string walletType);
event TransactionExecuted(address indexed target, uint256 value, bytes data, bool success);
event OwnerAdded(bytes indexed ownerData);
event OwnerRemoved(bytes indexed ownerData);
event EmergencyModeActivated();
```

## Testing

```solidity
// Test wallet creation
function testCreateWallet() public {
  bytes[] memory owners = new bytes[](1);
  owners[0] = abi.encode(x, y);
  
  IWallet wallet = factory.createWallet(owners, "test", 1);
  
  assertEq(wallet.ownerCount(), 1);
}

// Test execution
function testExecute() public {
  vm.prank(owner);
  (bool success,) = wallet.execute(target, 0, data);
  assertTrue(success);
}
```

## Gas Costs

Typical costs on Base:
- Wallet deployment: ~0.5M gas (~$0.01)
- Execute transaction: ~100k gas (~$0.001)
- Add owner: ~50k gas (~$0.0005)

## Resources

- [ERC-4337 Spec](https://eips.ethereum.org/EIPS/eip-4337)
- [Coinbase Smart Wallet](https://github.com/coinbase/smart-wallet)
- [Contract Addresses](/reference/contract-addresses.md)
