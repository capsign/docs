# Security Best Practices

Keep your CapSign account secure with these best practices and security guidelines.

## Core Security Principles

### You Are in Control

CapSign is **non-custodial**, meaning:

- You control your account through your passkey
- Your private key never leaves your device
- CapSign cannot access your funds or account
- You are responsible for your account security

### Multi-Layer Security

Your account is protected by multiple layers:

1. **Passkey** - Cryptographic key pair
2. **Biometric** - Face ID, Touch ID, or Windows Hello
3. **Device Security** - Device lock and encryption
4. **Smart Contract** - Programmable security rules
5. **Monitoring** - Unusual activity detection

### Security is a Habit

Security requires ongoing attention:

- Regularly review settings
- Monitor account activity
- Keep devices updated
- Stay informed about threats
- Practice security habits

## Device Security

### Secure Your Devices

**Physical Security**:
- Keep devices with you or locked away
- Use device lock (PIN, password, biometric)
- Enable "Find My Device" features
- Report lost/stolen devices immediately

**Software Security**:
- Keep OS updated
- Install security updates promptly
- Use official app stores only
- Enable auto-updates
- Use reputable security software

**Network Security**:
- Use secure Wi-Fi networks
- Avoid public Wi-Fi for sensitive operations
- Use VPN on untrusted networks
- Be cautious with Bluetooth connections

### Biometric Authentication

**Best Practices**:
- Enable biometric lock on all devices
- Don't share biometric access (Face ID, fingerprints)
- Register only your own biometrics
- Use backup authentication (PIN) as fallback

**What to Avoid**:
- Don't let others use your biometrics "just once"
- Don't register other people's biometrics
- Don't disable biometric requirement
- Don't use weak backup PINs

## Passkey Security

### Protecting Your Passkey

**Do**:
- Let passkey sync automatically (iCloud/Google)
- Use strong device passwords/PINs
- Enable device encryption
- Keep recovery options secure

**Don't**:
- Never export or share your passkey
- Don't try to back up manually
- Don't store in insecure locations
- Don't share device access

### Passkey Sync

Your passkey syncs via:

**iOS/Mac** - iCloud Keychain:
- End-to-end encrypted
- Requires Apple ID
- 2FA on Apple ID required
- Sync across all your Apple devices

**Android/Chrome** - Google Password Manager:
- End-to-end encrypted
- Requires Google account
- 2FA on Google account required
- Sync across Android devices and Chrome

**Security Note**: Secure your iCloud/Google account with:
- Strong unique password
- Two-factor authentication
- Recovery options
- Regular security reviews

## Account Security

### Enable All Security Features

1. **Two-Factor Authentication**:
   - Go to **Settings** → **Security** → **2FA**
   - Enable additional authentication methods
   - Use authenticator app (Google Authenticator, Authy)
   - Save backup codes securely

2. **Account Recovery**:
   - Set up recovery email
   - Configure security questions
   - Add recovery guardians
   - Store recovery information securely

3. **Transaction Limits**:
   - Set daily transaction limits
   - Require additional approval for large amounts
   - Enable time delays for large transfers

4. **Notifications**:
   - Enable all security notifications
   - Set up transaction alerts
   - Monitor login notifications
   - Review unusual activity alerts

### Regular Security Reviews

**Weekly**:
- Review recent transactions
- Check for unauthorized access
- Verify connected devices
- Monitor notification settings

**Monthly**:
- Full security audit
- Update recovery information
- Review connected applications
- Check for security updates

**Quarterly**:
- Change security questions
- Review and revoke unused permissions
- Update contact information
- Verify attestations are current

## Transaction Security

### Before Every Transaction

**Verify**:
- Recipient address is correct
- Amount is accurate
- Network is correct (Base, Ethereum, etc.)
- Gas fees are reasonable
- You initiated this transaction

**Check**:
- Biometric prompt is from CapSign
- Website URL is correct (app.capsign.com)
- No browser warnings
- Transaction details match intention

### During Transactions

**Best Practices**:
- Review all details carefully
- Take your time - don't rush
- If something seems wrong, cancel
- Use test transactions for large amounts
- Keep transaction confirmations

**Red Flags**:
- Unexpected biometric prompts
- Unusual transaction details
- Requests for immediate action
- Pressure to complete quickly
- Unfamiliar addresses

### After Transactions

- Save transaction hash
- Verify on block explorer
- Keep confirmation for records
- Monitor for confirmation
- Watch for completion notification

## Recognizing Threats

### Phishing Attacks

**Common Tactics**:
- Fake CapSign websites
- Emails claiming to be from CapSign
- Messages asking for passkey or biometric
- Links to malicious websites
- Urgent security warnings

**How to Recognize**:
- Check URL carefully (app.capsign.com only)
- Look for HTTPS and valid certificate
- Verify email sender
- Be suspicious of urgency
- CapSign never asks for passkeys

**What to Do**:
- Don't click suspicious links
- Report to security@capsign.com
- Never share passkey or biometric
- Verify requests through official channels

### Social Engineering

**Common Scams**:
- Impersonating CapSign support
- Pretending to be team members
- Offering "help" with account issues
- Requesting remote access to device
- Asking for verification information

**Protect Yourself**:
- Verify identity through official channels
- Never give remote device access
- CapSign support won't ask for passkeys
- Be wary of unsolicited help
- Report suspicious contacts

### Malware and Keyloggers

**Risks**:
- Screen capture malware
- Keylogging software
- Clipboard hijacking
- Session hijacking
- Device compromise

**Prevention**:
- Use reputable security software
- Keep software updated
- Don't install unknown applications
- Be cautious with downloads
- Use official app stores

## Privacy Best Practices

### Protect Your Information

**Personal Information**:
- Share only when necessary
- Use privacy settings
- Monitor attestation sharing
- Review application permissions
- Be cautious on social media

**Wallet Address**:
- Your address is public (blockchain)
- Anyone can see transactions
- Consider using multiple accounts
- Separate personal and public activity

**Transaction Privacy**:
- Blockchain transactions are public
- Amounts and addresses visible
- Use privacy features when available
- Be aware of on-chain footprint

### Managing Permissions

**Review Regularly**:
1. **Settings** → **Privacy** → **Permissions**
2. Check which applications have access
3. Review attestation sharing
4. Revoke unused permissions
5. Audit data sharing

**Grant Minimal Access**:
- Only give necessary permissions
- Use time-limited access when possible
- Revoke when no longer needed
- Question excessive requests

## Recovery and Backup

### Setting Up Recovery

**Critical**: Set up recovery BEFORE you need it

1. **Recovery Email**:
   - Add alternative email
   - Must be secure and accessible
   - Used for account recovery

2. **Social Recovery**:
   - Add trusted guardians
   - Choose people you trust
   - They can help recover access
   - Requires multiple guardians

3. **Security Questions**:
   - Choose memorable questions
   - Use unique answers
   - Don't use public information
   - Store answers securely

### What to Backup

**Do Backup**:
- Transaction history (export CSV)
- Important transaction hashes
- Attestation information
- Document copies
- Recovery codes (offline)

**Don't Try to Backup**:
- Passkey (syncs automatically)
- Private keys (in device secure enclave)
- Biometric data (can't be exported)

### If You Lose Access

1. **Try Alternative Devices**:
   - Check other synced devices
   - Use backup authentication
   - Try recovery email

2. **Use Recovery Process**:
   - Contact support@capsign.com
   - Provide identity verification
   - Use recovery options you set up
   - Follow recovery procedure

3. **Prevention**:
   - Set up recovery before needed
   - Keep multiple devices synced
   - Maintain current recovery info
   - Test recovery process

## Common Security Mistakes

### What NOT to Do

1. **Never**:
   - Share your passkey
   - Share device biometric access
   - Click suspicious links
   - Install untrusted software
   - Ignore security warnings
   - Use weak device passwords
   - Share recovery information publicly

2. **Avoid**:
   - Using public computers for CapSign
   - Using public Wi-Fi without VPN
   - Ignoring software updates
   - Disabling security features
   - Reusing passwords across services
   - Clicking email links (type URL instead)

3. **Don't Ignore**:
   - Unusual activity notifications
   - Security alerts
   - Failed login attempts
   - Unauthorized transactions
   - Unexpected biometric prompts

## Incident Response

### If You Suspect Compromise

**Immediate Actions**:

1. **Secure Your Account**:
   - Change device passwords
   - Sign out all devices
   - Enable 2FA if not already
   - Review recent transactions

2. **Assess the Situation**:
   - Check transaction history
   - Look for unauthorized activity
   - Review connected devices
   - Check application permissions

3. **Contact Support**:
   - Email security@capsign.com immediately
   - Provide details of incident
   - Include relevant transaction hashes
   - Follow instructions from team

4. **Secure Other Accounts**:
   - Change passwords on related accounts
   - Enable 2FA everywhere
   - Monitor for unusual activity
   - Alert your bank if needed

### After an Incident

**Learn and Improve**:
- Understand what happened
- Identify security gaps
- Implement stronger measures
- Update security practices
- Share learnings (without compromising security)

## Staying Informed

### Security Resources

- **[Security Blog](https://blog.capsign.com/security)** - Security updates
- **[Discord #security](https://discord.gg/gSmnZ9wmNv)** - Community alerts
- **[Twitter @capsign](https://twitter.com/capsign)** - Announcements
- **Email Notifications** - Account security alerts

### Regular Education

- Read security updates
- Participate in security webinars
- Follow blockchain security news
- Learn about new threats
- Share knowledge with others

## Getting Help

### Security Assistance

- **Urgent Security Issues**: security@capsign.com
- **General Support**: support@capsign.com
- **Discord**: [Community](https://discord.gg/gSmnZ9wmNv)
- **FAQ**: [Security questions](/getting-started/faq.md#security--privacy)

### Reporting Security Issues

Found a security vulnerability?

1. **Do NOT** post publicly
2. Email security@capsign.com
3. Provide details confidentially
4. Allow time for fix
5. Responsible disclosure

We appreciate security researchers helping keep CapSign secure.

---

**Remember**: Security is your responsibility. Stay vigilant, follow best practices, and don't hesitate to ask for help.

