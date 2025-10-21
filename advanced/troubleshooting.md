# Troubleshooting

Common issues and solutions when using CapSign.

## Authentication Issues

### Passkey Creation Fails

**Error**: "WebAuthn not supported"

**Solutions**:
- Use supported browser (Chrome, Safari, Edge, Firefox)
- Ensure HTTPS (or localhost for development)
- Check device has biometric authentication
- Update browser to latest version

### Can't Sign In

**Error**: Various authentication failures

**Solutions**:
- Ensure passkey exists on device or synced device
- Try "Use a different passkey"
- Check browser has passkey access
- Use account recovery if needed

## Transaction Issues

### Transaction Failed

**Error**: UserOperation reverted

**Solutions**:
- Check wallet has sufficient balance
- Verify network connection
- Ensure correct network selected (Base, Ethereum)
- Check contract hasn't been paused
- Review transaction details for errors

### Gas Estimation Failed

**Error**: Cannot estimate gas

**Solutions**:
- Check transaction is valid
- Verify contract addresses are correct
- Ensure contract functions exist
- Try with manual gas limits

### Wrong Wallet Address

**Error**: Using individual wallet in entity context

**Solutions**:
- Always use `createContextSmartAccount()`
- Never import adapters directly
- Check current context before operations
- Verify session has correct context

## Wallet Issues

### Wallet Not Deployed

**Error**: "Wallet not deployed"

**Solutions**:
- Deploy wallet first (automatic on first transaction)
- Check deployment transaction confirmed
- Verify CREATE2 address calculation
- Ensure sufficient ETH for deployment

### MetaMask Not Connecting (Entity Context)

**Error**: Cannot connect external wallet

**Solutions**:
- Install MetaMask or compatible wallet
- Unlock wallet in extension
- Switch to correct account
- Refresh page after connecting

## Attestation Issues

### Attestation Not Found

**Error**: Cannot load attestations

**Solutions**:
- Verify wallet address correct
- Check attestation was issued
- Ensure using correct network
- Wait for blockchain confirmation

### Verification Failed

**Error**: Attestation verification failed

**Solutions**:
- Check attestation not revoked
- Verify not expired
- Confirm correct schema
- Contact issuer if persistent

## Development Issues

### TypeScript Errors

**Error**: Type mismatches

**Solutions**:
- Run `pnpm install` to update dependencies
- Check viem and wagmi versions compatible
- Verify imports from correct packages
- Clear `.next` and rebuild

### Environment Variables

**Error**: Missing configuration

**Solutions**:
- Copy `.env.example` to `.env.local`
- Fill in all required variables
- Restart development server
- Check variable names match exactly

### Database Errors

**Error**: Prisma client errors

**Solutions**:
- Run `pnpm prisma generate`
- Run `pnpm prisma migrate dev`
- Check DATABASE_URL is correct
- Verify database is running

## Getting Help

If issues persist:

1. **Check Documentation**: Search docs for your specific error
2. **Discord**: [Join community](https://discord.gg/gSmnZ9wmNv)
3. **GitHub Issues**: [Report bugs](https://github.com/capsign)
4. **Email Support**: support@capsign.com

### Reporting Issues

When reporting, include:

- Error message (full text)
- Steps to reproduce
- Browser and OS version
- Network (testnet/mainnet)
- Transaction hash (if applicable)
- Screenshots if relevant

---

**Return to:** [Advanced Topics](/advanced/) | [Documentation Home](/)

