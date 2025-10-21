# Key Concepts

Understanding these core concepts will help you get the most out of CapSign.

## Smart Accounts

### What is a Smart Account?

A **smart account** (also called a smart contract wallet) is a blockchain account controlled by programmable smart contract code rather than a single private key.

### Traditional Wallet vs Smart Account

| Feature | Traditional Wallet (EOA) | Smart Account |
|---------|--------------------------|---------------|
| **Control** | Single private key | Programmable logic |
| **Recovery** | Seed phrase only | Social recovery, guardians |
| **Authentication** | Private key signature | Multiple methods (passkeys, biometrics) |
| **Gas Fees** | Must hold ETH | Can be sponsored (gasless) |
| **Security** | Basic | Multi-factor, spending limits |
| **Complexity** | Simple | Advanced features |

### Benefits of Smart Accounts

1. **No Seed Phrases**
   - Traditional wallets require managing 12-24 word seed phrases
   - Losing your seed phrase means losing your assets forever
   - Smart accounts use alternative recovery methods

2. **Biometric Authentication**
   - Use Face ID or Touch ID instead of complex passwords
   - Passkeys stored in device secure enclave
   - More convenient and more secure

3. **Account Recovery**
   - Recover access through trusted guardians
   - Social recovery mechanisms
   - Time-delayed recovery for security

4. **Gasless Transactions**
   - Platform can sponsor gas fees for common operations
   - No need to hold ETH for gas
   - Better user experience

5. **Programmable Security**
   - Spending limits
   - Transaction delays for large transfers
   - Multi-signature requirements
   - Whitelist addresses

### How CapSign Uses Smart Accounts

CapSign implements smart accounts using the **ERC-4337** standard:

- **Account Abstraction** - Separates account logic from validation
- **UserOperations** - Bundled transactions for efficiency
- **Paymasters** - Enable gasless transactions
- **Entry Point** - Standardized contract for account operations

Your CapSign wallet is deployed as a smart contract when you create your account. This contract:
- Validates signatures from your passkey
- Manages permissions and recovery
- Interacts with CapSign protocol contracts
- Holds your assets and attestations

## Passkeys

### What are Passkeys?

**Passkeys** are a modern authentication standard that replace passwords with cryptographic key pairs:

- **Private key** - Stored in your device's secure enclave, never leaves the device
- **Public key** - Shared with CapSign to verify your signatures
- **Biometric unlock** - Access protected by Face ID or Touch ID

### How Passkeys Work

1. **Registration**
   - You create a passkey when signing up
   - Your device generates a cryptographic key pair
   - Private key stored in secure enclave (Secure Enclave on iOS, TEE on Android)
   - Public key registered with CapSign

2. **Authentication**
   - When you sign in, CapSign challenges your device
   - Device prompts for biometric authentication
   - Private key signs the challenge (never leaves device)
   - CapSign verifies signature with your public key

3. **Cross-Device Sync**
   - Passkeys sync via iCloud Keychain (Apple) or Google Password Manager
   - End-to-end encrypted during sync
   - Available across all your devices

### Benefits of Passkeys

| Benefit | Description |
|---------|-------------|
| **Phishing Resistant** | Can't be stolen through phishing websites |
| **No Passwords** | Nothing to remember or type |
| **Device Bound** | Private key never leaves your device |
| **Biometric Protected** | Requires Face ID or Touch ID |
| **Cross-Platform** | Works across devices and platforms |
| **Standard Based** | WebAuthn standard, widely supported |

### Passkeys vs Traditional Methods

| Method | Security | Convenience | Recovery |
|--------|----------|-------------|----------|
| **Passwords** | Weak (reuse, phishing) | Medium | Easy |
| **Seed Phrases** | Strong (if stored safely) | Poor | Impossible if lost |
| **Passkeys** | Very Strong | Excellent | Via device sync |

### CapSign Passkey Implementation

When you create a CapSign account:

1. **Passkey Creation**
   - WebAuthn API generates passkey
   - Private key stored in device secure enclave
   - Public key registered with your account

2. **Smart Wallet Derivation**
   - Your smart account address is derived from passkey public key
   - Deterministic derivation ensures consistency
   - Same passkey always yields same wallet address

3. **Transaction Signing**
   - Every action requires biometric confirmation
   - Passkey signs transaction via WebAuthn
   - Signature verified by smart account contract

4. **Database + Cache Strategy**
   - Public key stored in database (source of truth)
   - Cached locally for performance
   - 7-day cache expiration

Learn more: [Interface Documentation - Passkeys](/interface/passkeys.md)

## Attestations

### What are Attestations?

**Attestations** are on-chain credentials that prove facts about you or your account:

- Created on Ethereum Attestation Service (EAS)
- Cryptographically signed by trusted issuers
- Stored on-chain for transparency
- Privacy-preserving (no personal data on-chain)

### How Attestations Work

1. **Credential Issuance**
   - You complete verification process (e.g., KYC)
   - Trusted issuer verifies your information off-chain
   - Issuer creates attestation on EAS
   - Attestation references a schema (defines data structure)

2. **Attestation Structure**
   ```
   Attestation {
     recipient: your wallet address
     attester: issuer's address
     schema: attestation type (KYC, accreditation, etc.)
     data: encoded credential data
     timestamp: when issued
     revocable: can it be revoked?
   }
   ```

3. **Verification**
   - Anyone can verify attestation on-chain
   - Check attester is trusted
   - Verify attestation hasn't been revoked
   - No personal data revealed

### Types of Attestations in CapSign

#### Identity Attestations

**KYC Attestation** - Proves you completed identity verification
- Schema: KYC verification schema
- Data: verification level, timestamp, jurisdiction
- Issuer: CapSign or third-party KYC provider

**Age Attestation** - Proves you meet age requirements
- Schema: Age verification schema
- Data: minimum age met (true/false), timestamp
- No birthdate revealed

**Geographic Attestation** - Proves your jurisdiction
- Schema: Geographic location schema
- Data: country code, region
- Used for compliance restrictions

#### Qualification Attestations

**Accredited Investor** - Proves accredited investor status
- Schema: Accreditation schema
- Data: accreditation type, expiration date
- Required for private securities

**Professional Credentials** - Proves professional qualifications
- Schema: Professional license schema
- Data: license type, issuing authority
- For regulated activities

#### Custom Attestations

Developers can create custom attestation schemas for:
- Organizational membership
- Course completion certificates
- Reputation scores
- Social credentials
- Access permissions

### Benefits of Attestations

1. **Privacy Preserving**
   - Attestation proves a fact without revealing underlying data
   - "User is over 18" without revealing birthdate
   - "User is accredited" without revealing financial details

2. **Portable**
   - Attestations follow your wallet address
   - Use attestations across multiple applications
   - No need to verify identity repeatedly

3. **Transparent**
   - Anyone can verify attestation authenticity
   - Audit trail on blockchain
   - See all attestations for any address

4. **Revocable**
   - Issuer can revoke if credential expires
   - Real-time verification status
   - No invalid credentials lingering

### Using Attestations

#### As a User

1. **View Your Attestations**
   - Navigate to Dashboard → Attestations
   - See all attestations linked to your wallet
   - Check issuer, date, and status

2. **Share Attestations**
   - Grant applications access to verify your attestations
   - No personal data shared
   - Revocable access

3. **Maintain Attestations**
   - Complete verification processes to obtain new attestations
   - Monitor expiration dates
   - Renew when needed

#### As a Developer

1. **Verify Attestations**
   ```solidity
   function hasKYC(address user) public view returns (bool) {
       bytes32 attestationUID = getAttestation(user, KYC_SCHEMA);
       return attestationUID != bytes32(0);
   }
   ```

2. **Create Custom Schemas**
   - Define schema structure
   - Register with EAS
   - Issue attestations to users

3. **Build Gated Features**
   - Require specific attestations for access
   - Verify on-chain in real-time
   - Enforce compliance automatically

Learn more: [Protocol Documentation - Attestations](/protocol/attestations.md)

## Diamond Pattern Architecture

### What is the Diamond Pattern?

The **Diamond Pattern** (EIP-2535) is a smart contract architecture that enables:

- **Modularity** - Functionality split into separate facets
- **Upgradeability** - Add/remove/replace features without redeploying
- **No Size Limit** - Bypass 24KB contract size limit
- **Single Address** - All functionality accessible via one address

### How Diamonds Work

```
            Diamond (Proxy)
                  |
    +-------------+-------------+
    |             |             |
  Facet A      Facet B      Facet C
  (Users)    (Transfers)   (Metadata)
```

- **Diamond** - Main contract with storage and delegation logic
- **Facets** - Individual contracts containing specific functionality
- **DiamondCut** - Function to add/remove/replace facets
- **Loupe** - Functions to inspect diamond configuration

### Why CapSign Uses Diamonds

1. **Protocol Upgrades**
   - Add new features without migrating users
   - Fix bugs in specific facets
   - Maintain same contract address

2. **Modularity**
   - Separate concerns (authentication, transfers, attestations)
   - Easier testing and auditing
   - Cleaner code organization

3. **Gas Efficiency**
   - Only deploy facets you need
   - Share facets across multiple diamonds
   - Optimize specific functions independently

4. **Factory Pattern**
   - Deploy custom diamonds with different facet combinations
   - User-specific wallets with tailored functionality
   - Governance structures with modular features

Learn more: [Protocol Documentation - Architecture](/protocol/architecture.md)

## Next Steps

Now that you understand the core concepts:

### For Users
- **[Managing Your Wallet](/guides/managing-your-wallet.md)** - Wallet operations guide
- **[Viewing Attestations](/guides/viewing-attestations.md)** - Understanding your credentials
- **[Security Best Practices](/guides/security-best-practices.md)** - Keep your account secure

### For Developers
- **[Protocol Overview](/protocol/README.md)** - Deep dive into smart contracts
- **[Interface Documentation](/interface/README.md)** - Frontend integration guide
- **[Smart Accounts](/protocol/smart-accounts.md)** - ERC-4337 implementation details
- **[Attestations](/protocol/attestations.md)** - Attestation system architecture

---

**Questions?** Check our **[FAQ](/getting-started/faq.md)** or join our **[Discord](https://discord.gg/gSmnZ9wmNv)**.

