# Managing Your Wallet

Learn how to manage your CapSign smart account wallet, view balances, send transactions, and track activity.

## Understanding Your Wallet

Your CapSign wallet is a **smart contract wallet** (ERC-4337) with advanced features:

- **Smart contract based**: Programmable functionality
- **Biometric protected**: Face ID/Touch ID required for transactions
- **Gasless transactions**: Platform sponsors gas for many operations
- **Multi-device access**: Available on all your synced devices
- **Recoverable**: Social recovery and guardian systems

Learn more: [Key Concepts - Smart Accounts](/getting-started/key-concepts.md#smart-accounts)

## Accessing Your Wallet

### Finding Your Wallet Information

1. Log in to CapSign
2. Your wallet address is displayed:
   - Dashboard header (abbreviated)
   - **Wallet** page (full address)
   - **Settings** → **Account**

### Your Wallet Address

Your wallet address is a unique identifier:
```
Example: 0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb
```

**Uses**:
- Receive tokens and assets
- Identify your account on blockchain
- Verify transactions on block explorers
- Share with others for receiving payments

**Security Note**: Your wallet address is public. Never share your passkey or biometric access.

## Viewing Balances

### Dashboard Overview

Your dashboard shows:

- **Total Balance**: Combined value of all assets (in USD)
- **Individual Tokens**: List of tokens you hold
- **Recent Activity**: Latest transactions
- **Pending Transactions**: In-progress operations

### Detailed Token View

1. Navigate to **Wallet** → **Balances**
2. For each token, you can see:
   - Token name and symbol
   - Amount held
   - Current value (USD)
   - 24h change
   - Token contract address

### Supported Assets

CapSign supports:
- **Native Tokens**: ETH (on Base)
- **ERC-20 Tokens**: Standard fungible tokens
- **ERC-721 NFTs**: Non-fungible tokens
- **ERC-1155 Tokens**: Multi-token standard
- **ERC-7752 Lot Tokens**: CapSign lot-based tokens

## Sending Transactions

### Sending Tokens

To send tokens:

1. Navigate to **Wallet** → **Send**
2. Enter recipient address
3. Select token to send
4. Enter amount
5. Review transaction details:
   - Recipient
   - Amount
   - Network fee (if applicable)
   - Total
6. Click **Send**
7. Confirm with biometric authentication
8. Transaction submitted

**Tips**:
- Double-check recipient address
- Start with a small test transaction
- Verify network (Base, Ethereum, etc.)
- Note that transactions are irreversible

### Transaction Fees

CapSign uses different fee models:

**Sponsored Transactions (Gasless)**:
- Many operations are sponsored by the platform
- No gas fees required
- Seamless user experience

**User-Paid Transactions**:
- Some operations require gas fees
- Fee displayed before confirmation
- Can be paid from wallet balance
- Fee varies by network congestion

### Transaction Speed

- **Base Network**: 1-2 seconds
- **Ethereum**: 15-30 seconds
- **Pending**: Transaction submitted, waiting for confirmation
- **Confirmed**: Transaction finalized on-chain

## Receiving Tokens

### Share Your Address

To receive tokens:

1. Navigate to **Wallet** → **Receive**
2. Your wallet address is displayed
3. Options:
   - **Copy Address**: Copy to clipboard
   - **Show QR Code**: Display QR for scanning
   - **Share**: Share via messaging apps

### QR Code

Use QR code for easy sharing:
- Contains your wallet address
- Can be scanned by other wallets
- Useful for in-person transfers

### Verifying Receipt

When expecting a payment:

1. Check **Wallet** → **Activity**
2. Look for incoming transaction
3. Verify amount and sender
4. Wait for confirmation (usually seconds on Base)

## Transaction History

### Viewing Activity

Access your transaction history:

1. Navigate to **Wallet** → **Activity**
2. See all transactions:
   - Sent
   - Received
   - Contract interactions
   - Failed transactions

### Transaction Details

Click any transaction to view:

- **Status**: Confirmed, pending, or failed
- **Type**: Send, receive, contract call
- **Amount**: Token amount transferred
- **From/To**: Addresses involved
- **Timestamp**: Date and time
- **Network Fee**: Gas paid
- **Transaction Hash**: Unique identifier
- **Block Number**: Block containing transaction

### Exporting History

Export your transaction history:

1. Go to **Wallet** → **Activity**
2. Click **Export**
3. Choose format:
   - CSV (for spreadsheets)
   - PDF (for reports)
   - JSON (for developers)
4. Select date range
5. Download file

**Uses**:
- Tax reporting
- Accounting records
- Personal tracking
- Audit trails

## Advanced Features

### Multiple Accounts (Context Switching)

CapSign supports multiple accounts:

1. Create additional accounts:
   - Personal account
   - Business account
   - Different purposes
2. Switch between accounts:
   - Dashboard dropdown
   - **Settings** → **Accounts**
3. Each account has:
   - Separate wallet address
   - Independent balances
   - Distinct attestations

Learn more: [Interface Documentation - Context Management](/interface/context-management.md)

### Connecting External Wallets

Connect traditional wallets (MetaMask, Coinbase Wallet):

1. Navigate to **Settings** → **Connected Wallets**
2. Click **Connect Wallet**
3. Choose wallet provider
4. Approve connection
5. Use for:
   - Additional signing
   - Asset transfers
   - Multi-sig operations

Learn more: [Interface Documentation - Direct Signing](/interface/direct-signing.md)

### Contract Interactions

Your smart account can interact with other contracts:

- **DeFi Protocols**: Lending, swapping, staking
- **NFT Marketplaces**: Buying and selling NFTs
- **CapSign Contracts**: Documents, attestations
- **Custom Contracts**: Any Ethereum-compatible contract

### Batch Transactions

Execute multiple operations in one transaction:

1. Navigate to **Wallet** → **Batch**
2. Add multiple operations:
   - Multiple sends
   - Multiple contract calls
   - Mixed operations
3. Review all operations
4. Execute with single biometric confirmation
5. All operations execute atomically (all or nothing)

## Security Best Practices

### Protecting Your Wallet

- **Never share**: Your passkey, biometric access, or private keys
- **Verify addresses**: Always double-check recipient addresses
- **Test first**: Send small amount first for large transfers
- **Keep devices secure**: Use device lock, keep OS updated
- **Enable security features**: 2FA, recovery options
- **Be cautious**: Watch for phishing attempts

### Recognizing Scams

**Red Flags**:
- Unsolicited messages asking for funds
- Promises of guaranteed returns
- Pressure to act quickly
- Requests for passkey or biometric access
- Suspicious links or attachments
- Too-good-to-be-true offers

**If Suspicious**:
- Don't click links
- Don't send funds
- Don't share information
- Report to support@capsign.com
- Join Discord for community warnings

### Account Recovery

If you lose access:

1. **Use recovery options**:
   - Social recovery guardians
   - Recovery email
   - Security questions
2. **Contact support**: support@capsign.com
3. **Verify identity**: Complete identity verification
4. **Regain access**: New passkey created

Set up recovery: **Settings** → **Security** → **Recovery**

## Monitoring and Alerts

### Transaction Notifications

Configure notifications:

1. Go to **Settings** → **Notifications**
2. Enable:
   - **Incoming transactions**: When you receive funds
   - **Outgoing transactions**: Confirmation of sends
   - **Large transactions**: Alert for amounts above threshold
   - **Failed transactions**: Notification of failures

### Balance Alerts

Set up balance monitoring:

1. Navigate to **Settings** → **Alerts**
2. Configure:
   - **Low balance**: Alert when balance drops below amount
   - **High balance**: Alert for unusually high balances
   - **Token arrivals**: Notify when new tokens received

### Security Alerts

Automatic security notifications for:

- New device access
- Password changes
- Recovery attempts
- Unusual activity
- Security setting changes

## Troubleshooting

### Transaction Failed

**Common Causes**:
- Insufficient balance for gas
- Network congestion
- Contract execution error
- Transaction timeout

**Solutions**:
- Ensure sufficient balance
- Try again with higher gas
- Check contract is working properly
- Contact support if persists

### Can't See Balance

**Solutions**:
- Refresh page
- Check network connection
- Verify correct network selected
- Try different browser
- Clear cache and retry

### Transaction Pending Too Long

**Solutions**:
- Check block explorer for status
- Wait for network congestion to clear
- Contact support if stuck > 1 hour
- May need to cancel and resubmit

### Address Shows as Invalid

**Solutions**:
- Verify address format (0x followed by 40 hexadecimal characters)
- Remove any spaces or special characters
- Confirm network compatibility
- Try copying address again

## Getting Help

Need wallet assistance?

- **[FAQ](/getting-started/faq.md#smart-accounts--wallets)** - Common wallet questions
- **[Discord](https://discord.gg/gSmnZ9wmNv)** - Community support
- **[Email](mailto:support@capsign.com)** - Direct support
- **[Troubleshooting](/advanced/troubleshooting.md)** - Technical issues

---

**Next:** [Viewing Attestations](/guides/viewing-attestations.md) →

