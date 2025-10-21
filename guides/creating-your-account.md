# Creating Your Account

This guide walks you through creating your CapSign account step by step.

## Prerequisites

Before starting, ensure you have:

- A device with **Face ID**, **Touch ID**, or **Windows Hello**
- A supported browser (Chrome, Safari, Edge, Firefox)
- A valid email address
- Stable internet connection

## Step-by-Step Account Creation

### Step 1: Visit CapSign

Navigate to [app.capsign.com](https://app.capsign.com)

### Step 2: Click "Sign Up"

On the homepage, click the "Sign Up" or "Get Started" button.

### Step 3: Enter Your Email

1. Enter your email address
2. This will be used for:
   - Account notifications
   - Verification emails
   - Account recovery
3. Click "Continue"

### Step 4: Create Your Passkey

Your browser will prompt you to create a passkey:

**On iOS/Mac:**
- A dialog will appear asking to create a passkey
- Authenticate with Face ID or Touch ID
- Your passkey is created and stored in iCloud Keychain

**On Android:**
- A dialog will appear asking to create a passkey
- Authenticate with fingerprint or face unlock
- Your passkey is stored in Google Password Manager

**On Windows:**
- Windows Hello dialog appears
- Use PIN, fingerprint, or face recognition
- Passkey is stored securely on your device

### Step 5: Smart Account Creation

Once your passkey is created:

1. CapSign automatically creates your smart account
2. Your wallet address is derived from your passkey
3. The smart contract wallet is deployed on-chain
4. You're logged in and ready to go

## Understanding What Was Created

### Your Passkey

- **Private key**: Stored in your device's secure enclave, never leaves device
- **Public key**: Registered with CapSign for authentication
- **Sync**: Automatically synced across your devices (iCloud/Google)

Learn more: [Key Concepts - Passkeys](/getting-started/key-concepts.md#passkeys)

### Your Smart Account

- **Smart contract wallet**: ERC-4337 account deployed on Base
- **Wallet address**: Unique identifier for your account
- **Biometric-protected**: Every action requires Face ID/Touch ID
- **Gasless**: Platform sponsors gas fees for many operations

Learn more: [Key Concepts - Smart Accounts](/getting-started/key-concepts.md#smart-accounts)

## Completing Your Profile

After creating your account, complete your profile:

### Basic Information

1. Navigate to **Settings** → **Profile**
2. Add:
   - Full name
   - Profile picture (optional)
   - Bio (optional)
   - Location (optional)

### Notification Preferences

1. Go to **Settings** → **Notifications**
2. Configure:
   - Email notifications
   - Push notifications (if enabled)
   - Transaction alerts
   - Security notifications

### Privacy Settings

1. Visit **Settings** → **Privacy**
2. Control:
   - Profile visibility
   - Attestation sharing
   - Activity visibility
   - Data sharing preferences

## Security Setup

### Enable Two-Factor Authentication

While passkeys are very secure, you can add additional security:

1. Go to **Settings** → **Security**
2. Enable 2FA:
   - Email OTP
   - Authenticator app (optional)
   - Security questions

### Set Up Recovery Options

Configure account recovery in case you lose access:

1. Navigate to **Settings** → **Security** → **Recovery**
2. Options:
   - **Social recovery**: Add trusted guardians
   - **Recovery email**: Alternative email address
   - **Recovery questions**: Security questions

### Review Connected Devices

Check which devices have access to your account:

1. Go to **Settings** → **Security** → **Devices**
2. View all devices with your passkey
3. Remove access from lost or old devices

## Next Steps

Now that your account is created:

### 1. Complete Identity Verification

Identity verification is required to access most CapSign features:

- Navigate to **Settings** → **Verification**
- Follow the KYC process
- Upload required documents

See: [Identity Verification Guide](/guides/identity-verification.md)

### 2. Explore Your Dashboard

Familiarize yourself with the platform:

- **Wallet**: View balances and transactions
- **Attestations**: See your credentials
- **Documents**: Access signed documents
- **Activity**: Review recent actions

### 3. Fund Your Wallet (Optional)

If you need to hold assets:

1. Navigate to **Wallet** → **Deposit**
2. Choose funding method:
   - Bank transfer
   - Cryptocurrency deposit
   - Credit/debit card (where available)
3. Follow the instructions

### 4. Learn More

- [Managing Your Wallet](/guides/managing-your-wallet.md)
- [Viewing Attestations](/guides/viewing-attestations.md)
- [Security Best Practices](/guides/security-best-practices.md)

## Troubleshooting

### Can't Create Passkey

**Issue**: Browser doesn't prompt for passkey creation

**Solutions**:
- Ensure browser supports WebAuthn (Chrome, Safari, Edge, Firefox)
- Check that biometric authentication is enabled on your device
- Try incognito/private mode to rule out extensions
- Clear browser cache and try again

### Passkey Not Syncing

**Issue**: Can't access account from another device

**Solutions**:
- **iOS**: Ensure iCloud Keychain is enabled in Settings
- **Android**: Check Google Password Manager is set up
- Wait a few minutes for sync to complete
- Try signing in again on the new device

### Email Not Received

**Issue**: Verification email not arriving

**Solutions**:
- Check spam/junk folder
- Ensure email address was entered correctly
- Wait a few minutes (can take up to 10 minutes)
- Request a new verification email
- Try a different email address

### Smart Account Creation Failed

**Issue**: Wallet deployment failed

**Solutions**:
- Check your internet connection
- Try again (sometimes network issues cause failures)
- Clear browser cache
- Contact support if persists

## Tips for Success

### Choosing a Device

- Use a device you'll regularly access
- Ensure biometric authentication works reliably
- Consider using your primary smartphone

### Email Best Practices

- Use an email you check regularly
- Consider using a dedicated email for financial accounts
- Ensure the email account is secure (strong password, 2FA)

### Security Recommendations

- Enable all available security features
- Set up recovery options immediately
- Keep your device's OS updated
- Never share your passkey or biometric access

## Getting Help

Need assistance with account creation?

- **[FAQ](/getting-started/faq.md)** - Common questions
- **[Discord](https://discord.gg/gSmnZ9wmNv)** - Community support
- **[Email](mailto:support@capsign.com)** - Direct support
- **[Troubleshooting](/advanced/troubleshooting.md)** - Technical issues

---

**Account created successfully?** Continue with [Identity Verification](/guides/identity-verification.md) →

