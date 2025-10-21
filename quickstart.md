# Quickstart Guide

Get started with CapSign in minutes. Create your smart wallet instantly with Face ID or Touch ID - no complex setup required. Identity verification can be completed later when you're ready to invest.

## Prerequisites

To create your account, you'll need:

- A device with **Face ID** or **Touch ID** (iOS, Mac, Android with biometric support)
- A stable **internet connection**

That's it! No email, no government ID - just your biometric authentication. You can start exploring immediately.

## Step 1: Create Your Account

Visit [app.capsign.com](https://app.capsign.com) to create your account.

### What Happens:

1. **Click "Sign Up"** or "Get Started"
2. **Set up biometric authentication**
   - Your device will prompt you to use Face ID or Touch ID
   - This creates a secure passkey stored in your device's secure enclave
3. **Create your smart wallet**
   - A smart account (ERC-4337) is automatically created
   - Your wallet address is derived from your passkey's public key
   - No seed phrases to manage!

### What is a Smart Account?

Your CapSign account uses a **smart contract wallet** that:
- Requires biometric authentication for all actions
- Supports gasless transactions (no need to hold ETH)
- Can be recovered through social recovery mechanisms
- Works seamlessly across multiple devices

**Time required**: 30 seconds

## Step 2: Explore the Platform

You can now explore CapSign and familiarize yourself with the interface. Many features are available immediately without identity verification.

### What You Can Do Without Verification

- View your wallet and address
- Explore the interface
- Read documentation
- Set up account preferences
- Add additional contexts (when invited to entities)

## Step 3: Complete Identity Verification (When Ready)

When you're ready to invest or access regulated features, complete identity verification.

### Why Identity Verification?

CapSign operates in private securities markets and requires KYC (Know Your Customer) compliance for investment activities. This protects both you and the platform while ensuring regulatory compliance.

### Verification Process:

1. **Navigate to Settings** → **Identity Verification**
2. **Complete verification through Persona**
   - CapSign uses Persona (via Bridge.xyz integration) for identity verification
   - Provide basic information and ID documents
   - Complete liveness check with selfie
3. **Automated verification**
   - Persona performs real-time identity checks
   - Results typically within 1-5 minutes
   - Manual review if needed: 1-24 hours

### What Happens Behind the Scenes:

- Your information is verified through **Persona** via **Bridge.xyz integration**
- An **attestation** is created on-chain via Ethereum Attestation Service (EAS)
- This attestation proves your identity without revealing personal details
- The attestation is linked to your wallet address

### Issuer-Issued Attestations

In addition to identity verification, **issuers can issue attestations** for their users directly through the CapSign UI:

- **Accredited Investor Status** - Issuers verify and attest to accreditation
- **Professional Credentials** - Custom attestations for qualifications
- **Custom Attestations** - Application-specific credentials

These issuer attestations are used primarily for investment qualification and compliance.

**Time required**: 5-10 minutes (plus review time)

**Note**: Verification is required for investment activities, but you can create your account and explore the platform without it.

## Step 4: Explore Multi-Entity Workflows

Once you're set up, CapSign supports complex organizational access patterns.

### Understanding Contexts

CapSign supports **multi-entity workflows** through context switching:

**Use Cases**:
- **Individual Investor**: Your personal investment account
- **Corporate Employee**: Access your company's investment entity
- **Fund Manager**: Manage multiple SPVs and fund entities
- **Multi-Role**: Switch between GP of Fund A, LP in Fund B, CFO of Corp C

**How It Works**:
1. Click the account context menu (top right)
2. Select the entity you want to access
3. All features now operate in that context
4. Switch instantly - no logging in and out

**Example**: You're a fund manager with 3 SPVs and also an LP in another fund:
- Context 1: Personal account (individual investor)
- Context 2: SPV Alpha (GP role)
- Context 3: SPV Beta (GP role)
- Context 4: SPV Gamma (GP role)
- Context 5: XYZ Fund (LP role)

Switch between all five from the context menu.

### Your Dashboard

After logging in, you'll see:

- **Current Context** - Which entity you're operating as
- **Wallet Balance** - Smart account balance for current context
- **Attestations** - Identity credentials (once verified)
- **Documents** - Signed and pending documents
- **Activity** - Recent transactions and actions

### Key Features to Try

1. **View Your Attestations**
   - See all identity attestations linked to your account
   - Share attestations with third parties
   - Understand what each attestation means

2. **Sign a Document**
   - Navigate to Documents
   - Upload or select a document
   - Sign with your biometric authentication
   - The signature is recorded on-chain

3. **Manage Your Wallet**
   - View your wallet address
   - Check balances across different tokens
   - Review transaction history

4. **Explore Settings**
   - Update profile information
   - Configure notification preferences
   - Manage connected accounts
   - Review security settings

## Understanding Key Concepts

### Smart Accounts

Your CapSign account is a **smart contract wallet** (ERC-4337), not a traditional externally owned account (EOA). This means:

- **Programmable** - Advanced features like recovery, spending limits, and multi-sig
- **Gasless** - Platform sponsors gas fees for certain operations
- **Secure** - Multi-factor authentication and advanced security features

Learn more: [Key Concepts - Smart Accounts](/getting-started/key-concepts.md#smart-accounts)

### Passkeys

Instead of passwords or seed phrases, CapSign uses **passkeys**:

- Stored in your device's secure enclave (never on servers)
- Protected by Face ID or Touch ID
- Can't be phished or stolen remotely
- Synced securely across your devices via iCloud Keychain or Google Password Manager

Learn more: [Key Concepts - Passkeys](/getting-started/key-concepts.md#passkeys)

### Attestations

**Attestations** are on-chain credentials that prove facts about you:

- Identity verification (KYC)
- Accredited investor status
- Geographic location
- Age verification
- Custom credentials

Learn more: [Key Concepts - Attestations](/getting-started/key-concepts.md#attestations)

## Next Steps

Now that you're set up, explore these guides:

### For Users
- **[Identity Verification Guide](/guides/identity-verification.md)** - Detailed KYC walkthrough
- **[Managing Your Wallet](/guides/managing-your-wallet.md)** - Wallet operations
- **[Viewing Attestations](/guides/viewing-attestations.md)** - Understanding your credentials
- **[Security Best Practices](/guides/security-best-practices.md)** - Keep your account safe

### For Developers
- **[Protocol Overview](/protocol/README.md)** - Understand the smart contracts
- **[Interface Documentation](/interface/README.md)** - Build on CapSign
- **[API Reference](/api-reference/README.md)** - Complete API documentation

## Troubleshooting

### Can't create account?
- Ensure your device supports Face ID or Touch ID
- Check that you're using a supported browser (Chrome, Safari, Edge)
- Clear browser cache and try again

### Verification taking too long?
- Most verifications complete within minutes
- Check your email for any requests for additional information
- Contact support if verification exceeds 24 hours

### Can't sign in?
- Ensure biometric authentication is enabled on your device
- Try the "Use a different passkey" option
- Contact support for account recovery assistance

## Getting Help

Need assistance? We're here to help:

- **[FAQ](/getting-started/faq.md)** - Common questions and answers
- **[Discord Community](https://discord.gg/gSmnZ9wmNv)** - Chat with users and team
- **[Email Support](mailto:support@capsign.com)** - Direct support
- **[Troubleshooting Guide](/advanced/troubleshooting.md)** - Technical issues

---

**Ready to start?** [Create your account now](https://app.capsign.com) →

