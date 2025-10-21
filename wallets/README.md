# Wallets

CapSign wallets are smart contract wallets (ERC-4337) that use biometric authentication instead of seed phrases.

## What is a Smart Wallet?

A smart wallet is a smart contract on the blockchain that acts as your account. Unlike traditional wallets (EOAs), smart wallets offer:

- **No seed phrases** - Your wallet is controlled by Face ID / Touch ID
- **Account abstraction** - Better UX with gasless transactions
- **Programmable** - Support for multi-sig, spending limits, and recovery
- **Secure** - Biometric authentication required for all actions

## Key Features

### Biometric Authentication

Your wallet is protected by your device's biometric authentication:

- **Face ID** (iOS, Mac)
- **Touch ID** (iOS, Mac)
- **Fingerprint** (Android)
- **Windows Hello** (Windows)

Every transaction requires biometric confirmation - no one can access your wallet without your biometrics.

### Multi-Entity Support

Switch between multiple organizational contexts from a single account:

- **Personal Account** - Your individual investment account
- **Corporate Accounts** - Company investment accounts you're authorized for
- **Fund Accounts** - Investment funds and SPVs you manage
- **Multiple Roles** - GP in Fund A, LP in Fund B, employee of Corp C

Learn more: [Multi-Entity Accounts](multi-entity-accounts.md)

### Account Abstraction (ERC-4337)

Your wallet uses ERC-4337 account abstraction:

- **Gas sponsorship** - Platform can sponsor gas for certain operations
- **Batch transactions** - Execute multiple actions in one transaction
- **Custom validation** - Flexible signature schemes (passkeys, multi-sig)
- **UserOperations** - Transactions submitted through bundlers

### Security Features

- **Biometric-only access** - No seed phrases to lose or steal
- **On-chain verification** - All actions verified by smart contract
- **Replay protection** - Signatures can't be reused
- **Domain separation** - Signatures bound to specific contracts

## Getting Started

### Create Your Wallet

1. Visit [app.capsign.com](https://app.capsign.com)
2. Click "Sign Up"
3. Authenticate with Face ID / Touch ID
4. Your wallet is created instantly

**Time required:** 30 seconds

Learn more: [Quickstart Guide](/quickstart.md)

### Your Wallet Address

Your wallet address is displayed in the top right of the app. It looks like:

```
0x1234...5678
```

This is your public identifier - safe to share with others.

## Wallet Operations

### Sending Transactions

All transactions require biometric authentication:

1. **Initiate action** (send tokens, sign document, etc.)
2. **Review details** in the confirmation dialog
3. **Authenticate** with Face ID / Touch ID
4. **Transaction submitted** to the network

### Receiving Funds

To receive funds, share your wallet address with the sender.

**Supported assets:**
- ETH (native)
- ERC-20 tokens (USDC, etc.)
- ERC-721 NFTs
- ERC-1155 tokens
- CapSign securities tokens

### Transaction History

View your transaction history in the app:

- Navigate to **Wallet** → **History**
- See all incoming and outgoing transactions
- Filter by token type
- Export for tax purposes

## Account Recovery

### Passkey Recovery

Your passkey is stored in your device's secure enclave and synced via:

- **iCloud Keychain** (Apple devices)
- **Google Password Manager** (Android devices)
- **Windows Hello** (Windows devices)

If you lose your device, you can recover access on a new device that's signed into the same account.

### Social Recovery

Coming soon: Designate trusted contacts who can help you recover your wallet if you lose access to all your devices.

## Multi-Entity Management

CapSign supports complex organizational structures through context switching:

**Example use cases:**
- Fund manager with multiple SPVs
- Employee with corporate investment account
- LP in multiple funds
- GP and LP in different funds

Learn more: [Multi-Entity Accounts](multi-entity-accounts.md)

## Advanced Features

### Delegation

Coming soon: Delegate transaction permissions to other wallets (e.g., allow an employee to sign documents on behalf of the company).

### Multi-Signature

Coming soon: Require multiple signatures for certain operations (e.g., company treasury requires 2 of 3 executives).

### Spending Limits

Coming soon: Set daily or per-transaction spending limits for added security.

## Technical Details

### Contract Architecture

CapSign wallets use the Diamond Pattern (EIP-2535):

- **Modular facets** - Functionality split into separate contracts
- **Upgradeable** - New features can be added without changing the wallet address
- **Gas efficient** - Singleton pattern reduces deployment costs

Learn more: [Wallet Architecture](/protocol/wallets.md)

### Deployed Contracts

See [Contract Addresses](/reference/contract-addresses.md) for wallet factory and facet addresses.

## FAQs

**Q: Can I use my own wallet (MetaMask, Coinbase Wallet)?**
A: Entity accounts can connect an EOA wallet as a signer. Individual accounts use CapSign smart wallets only.

**Q: Where are my private keys?**
A: Your wallet is controlled by a passkey stored in your device's secure enclave. There are no traditional "private keys."

**Q: What if I lose my device?**
A: Your passkey syncs via iCloud Keychain (Apple) or Google Password Manager (Android). Sign in on a new device to recover access.

**Q: Can someone steal my wallet if they steal my device?**
A: No. Biometric authentication is required, and biometrics can't be extracted or replicated.

**Q: What networks are supported?**
A: Base Mainnet and Base Sepolia testnet. See [Supported Networks](/reference/supported-networks.md).

## Need Help?

- **Email:** support@capsign.com
- **Twitter:** [@CapSignInc](https://twitter.com/CapSignInc)

