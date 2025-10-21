# Frequently Asked Questions

## General Questions

### What is CapSign?

CapSign is a blockchain-based platform for digital identity, smart accounts, and document management. It combines cutting-edge Web3 technology with user-friendly design to provide secure, verifiable credentials and document signing.

### Is CapSign free to use?

CapSign operates on blockchain networks which have transaction fees (gas fees). However, many operations are sponsored by the platform (gasless) so you don't need to hold cryptocurrency. Some premium features may have associated costs.

### What devices does CapSign support?

CapSign works on:
- **iOS** - iPhone and iPad with Face ID or Touch ID
- **Android** - Devices with biometric authentication
- **Desktop** - Mac with Touch ID, Windows Hello, or security keys
- **Browsers** - Chrome, Safari, Edge, Firefox (with WebAuthn support)

### Do I need to understand cryptocurrency or blockchain?

No! CapSign is designed to be user-friendly for everyone. You don't need to understand blockchain technology to use the platform. The complexity is abstracted away behind a familiar interface.

## Account & Authentication

### What is a passkey?

A passkey is a modern, password-free authentication method that uses cryptographic keys stored securely in your device. It's protected by your device's biometric authentication (Face ID, Touch ID) and is much more secure than traditional passwords.

Learn more: [Key Concepts - Passkeys](/getting-started/key-concepts.md#passkeys)

### What happens if I lose my device?

Your passkeys are synced across your devices via:
- **iCloud Keychain** (Apple devices)
- **Google Password Manager** (Android/Chrome)

If you lose your device, you can still access your account from your other synced devices. We also provide account recovery mechanisms for additional safety.

### Can I use CapSign on multiple devices?

Yes! Your passkeys automatically sync across your devices (iOS via iCloud, Android via Google). Once you sign in on a new device, it will have access to your account.

### How do I sign in from a new device?

1. Navigate to app.capsign.com on your new device
2. Click "Sign In"
3. Select "Use a passkey"
4. Choose your synced passkey or use a security key
5. Authenticate with biometrics

Your new device will now have full access to your account.

### What if I can't access my passkey?

If you can't access your passkey through any of your devices:

1. Contact our support team at support@capsign.com
2. We'll initiate account recovery process
3. You'll need to verify your identity
4. A new passkey will be created and linked to your account

## Identity Verification

### Why do I need to verify my identity?

CapSign operates in regulated markets and requires KYC (Know Your Customer) compliance. Identity verification:
- Protects you from fraud and identity theft
- Ensures compliance with financial regulations
- Enables access to regulated features
- Prevents money laundering and illegal activities

### What information do I need to provide?

You'll need:
- Full legal name
- Date of birth  
- Address
- Nationality
- Government-issued ID (passport, driver's license, or national ID)
- Proof of address (utility bill or bank statement)
- Selfie for liveness check

### How long does verification take?

- **Automated verification**: 1-5 minutes
- **Manual review** (if needed): 1-24 hours

You'll receive email notification when verification is complete.

### Is my personal information secure?

Yes. Your personal information is:
- Encrypted in transit and at rest
- Stored in secure, compliant data centers
- Never shared without your explicit consent
- Subject to strict privacy policies

Only a cryptographic attestation (not your personal data) is stored on the blockchain.

### What if my verification is rejected?

If verification fails:
1. Check your email for specific reasons
2. Ensure ID documents are clear and valid
3. Verify information matches exactly
4. Try again with corrected information
5. Contact support if issues persist

## Smart Accounts & Wallets

### What is a smart account?

A smart account is a blockchain account controlled by smart contract code rather than a single private key. This enables features like:
- Biometric authentication
- Gasless transactions
- Account recovery
- Programmable security rules

Learn more: [Key Concepts - Smart Accounts](/getting-started/key-concepts.md#smart-accounts)

### Do I need to hold ETH for gas fees?

No! CapSign uses **account abstraction** (ERC-4337) which allows the platform to sponsor gas fees for many operations. This means you can use the platform without needing to buy or hold cryptocurrency.

### What is my wallet address?

Your wallet address is a unique identifier for your smart account on the blockchain. It's derived from your passkey's public key. You can find it in:
- Dashboard header
- Settings → Account
- Wallet page

### Can I use my existing wallet with CapSign?

CapSign creates a new smart account for you. While you can't directly "import" a traditional wallet, you can:
- Link an external wallet (MetaMask, Coinbase Wallet) for signing
- Transfer assets between wallets
- Use your external wallet as a signer for your smart account

Learn more: [Interface Documentation - Direct Signing](/interface/direct-signing.md)

### How do I back up my wallet?

Your smart account is backed up through:
1. **Passkey sync** - Automatic via iCloud/Google
2. **Account recovery** - Through our recovery mechanisms
3. **Guardian system** - Trusted contacts can help recover access

Unlike traditional wallets, there's no seed phrase to write down or lose.

## Attestations

### What is an attestation?

An attestation is a cryptographic credential stored on the blockchain that proves a fact about you (e.g., identity verified, over 18, accredited investor). It's issued by a trusted party and can be verified by anyone.

Learn more: [Key Concepts - Attestations](/getting-started/key-concepts.md#attestations)

### Who can see my attestations?

Attestations are stored on a public blockchain, so anyone can see that:
- Your wallet address has certain attestations
- Who issued them
- When they were issued

However, your personal information (name, ID documents, etc.) is NOT on the blockchain. Attestations only prove facts, not reveal details.

### Can I revoke or hide attestations?

You cannot delete attestations (they're on the blockchain), but:
- Issuers can **revoke** attestations
- You can choose which applications can **read** your attestations
- Revoked attestations show as invalid

### How long do attestations last?

It depends on the attestation type:
- **KYC attestations** - Typically 1 year, then need renewal
- **Age attestations** - Usually permanent
- **Accreditation** - Usually 1 year
- **Custom attestations** - Defined by issuer

You can check expiration dates in your Attestations dashboard.

## Documents

### What types of documents can I sign?

Any document type:
- PDFs
- Contracts and agreements
- Compliance forms
- Applications
- Certificates
- Custom documents

### Is document signing legally binding?

Document signing on CapSign uses cryptographic signatures that are:
- Legally recognized in many jurisdictions
- Compliant with e-signature laws (e.g., ESIGN Act, eIDAS)
- Verifiable on the blockchain
- Time-stamped and immutable

Consult local legal counsel for specific jurisdictions.

### How do I verify a signed document?

To verify a document signature:
1. Get the document hash (cryptographic fingerprint)
2. Check the blockchain for signature record
3. Verify signer's wallet address
4. Confirm timestamp and attestations

The verification process proves:
- Document hasn't been altered
- Signature is authentic
- Signer had proper attestations at signing time

## Technical Questions

### What blockchain does CapSign use?

CapSign primarily uses:
- **Base** - Layer 2 Ethereum network (low fees, fast)
- **Ethereum** - For cross-chain operations

### Can I see my transactions on a block explorer?

Yes! Use:
- **Base**: [basescan.org](https://basescan.org)
- **Ethereum**: [etherscan.io](https://etherscan.io)

Enter your wallet address to see all transactions.

### Is the code open source?

CapSign smart contracts and much of the platform code are open source under the Business Source License (BUSL-1.1). You can review code at:
- [github.com/capsign/protocol](https://github.com/capsign/protocol) - Smart contracts
- [github.com/capsign/interface](https://github.com/capsign/interface) - Frontend application

### How do I integrate CapSign into my app?

See our developer documentation:
- **[Interface Documentation](/interface/README.md)** - Frontend integration
- **[Protocol Documentation](/protocol/README.md)** - Smart contract integration
- **[API Reference](/api-reference/README.md)** - Complete API docs

## Security & Privacy

### How secure is CapSign?

CapSign security includes:
- **Passkeys** - Phishing-resistant, device-bound authentication
- **Smart contracts** - Audited code with security best practices
- **Biometric auth** - Face ID/Touch ID required for all actions
- **Encrypted storage** - All sensitive data encrypted
- **On-chain transparency** - Verifiable transactions

### What if CapSign gets hacked?

Your assets are protected because:
- Private keys never leave your device
- Smart contract code is immutable and audited
- Biometric authentication required for transactions
- On-chain security isn't dependent on CapSign servers

Even if CapSign infrastructure is compromised, your funds remain secure.

### Can CapSign access my wallet?

No. CapSign cannot:
- Access your private keys (stored in your device)
- Sign transactions on your behalf
- Move your funds without your biometric approval

CapSign is **non-custodial** - you maintain full control.

### How is my personal data used?

Your personal data is used only for:
- Identity verification
- Compliance with regulations
- Platform functionality
- Security and fraud prevention

We never sell your data to third parties. See our [Privacy Policy](https://capsign.com/privacy) for details.

## Troubleshooting

### I can't create an account

Common issues:
- **Device doesn't support biometrics** - Use a device with Face ID, Touch ID, or Windows Hello
- **Browser not supported** - Use Chrome, Safari, Edge, or Firefox
- **Cookies/storage disabled** - Enable cookies and local storage
- **Security software blocking** - Temporarily disable ad blockers

### Verification is stuck

If verification is taking too long:
1. Check email for requests for additional information
2. Ensure documents are clear and valid
3. Wait up to 24 hours for manual review
4. Contact support if exceeds 24 hours: support@capsign.com

### Transaction failed

Transaction failures can occur due to:
- Network congestion
- Insufficient gas (rare with sponsored transactions)
- Contract execution error
- Rejected by user

Check transaction on block explorer for specific error message.

### I see an error message

For specific error messages:
1. Note the exact error text
2. Check [Troubleshooting Guide](/advanced/troubleshooting.md)
3. Search our [Discord](https://discord.gg/gSmnZ9wmNv) for similar issues
4. Contact support with error details

## Getting Help

### How do I contact support?

Multiple channels:
- **Email**: support@capsign.com
- **Discord**: [discord.gg/gSmnZ9wmNv](https://discord.gg/gSmnZ9wmNv)
- **Twitter**: [@capsign](https://twitter.com/capsign)
- **Documentation**: [docs.capsign.com](https://docs.capsign.com)

### Where can I learn more?

- **[Quickstart Guide](/getting-started/quickstart.md)** - Get started quickly
- **[Key Concepts](/getting-started/key-concepts.md)** - Understand core concepts
- **[User Guides](/guides/)** - Step-by-step instructions
- **[Protocol Docs](/protocol/README.md)** - Technical deep dives

### How can I provide feedback?

We love feedback! You can:
- Join our [Discord community](https://discord.gg/gSmnZ9wmNv)
- Email us at feedback@capsign.com
- Submit issues on [GitHub](https://github.com/capsign)
- Fill out in-app feedback forms

---

**Still have questions?** Join our [Discord community](https://discord.gg/gSmnZ9wmNv) or email [support@capsign.com](mailto:support@capsign.com)

