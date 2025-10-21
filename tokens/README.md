# Tokens

CapSign tokens are ERC-7752 securities tokens with built-in lot-based accounting for precise capital gains tracking.

## What is a CapSign Token?

A CapSign token is a digital security that represents:

- **Shares** - Common stock, preferred stock
- **Units** - LLC membership units, partnership interests
- **Other securities** - Convertible notes, warrants, etc.

Unlike traditional ERC-20 tokens, CapSign tokens track **lots** - instances of tokens with the a unique acquisition date and cost basis.

## Key Features

### Lot-Based Accounting (ERC-7752)

Every token has a **lot** that tracks:

- **Acquisition date** - When you acquired the tokens
- **Cost basis** - What you paid per token
- **Payment currency** - What currency you used (USDC, ETH, etc.)
- **Quantity** - How many tokens in this lot
- **Transfer type** - How you acquired them (purchase, gift, etc.)

This enables:

- **Precise capital gains** - Calculate gains on a per-lot basis
- **Tax optimization** - Choose which lots to sell (FIFO, LIFO, specific ID)
- **Audit trails** - Complete transaction history for compliance
- **Regulatory compliance** - Meet securities reporting requirements

### Transfer Restrictions

Tokens can have compliance conditions:

- **Lockup periods** - Tokens can't be transferred until a date
- **Vesting schedules** - Tokens unlock over time
- **Rule 144** - SEC holding period requirements
- **ROFR** - Right of first refusal for transfers
- **Custom rules** - Issuers can define custom restrictions

### On-Chain Metadata

Token metadata includes:

- **Token name** - e.g., "Class A Common Stock"
- **Token symbol** - e.g., "CSA"
- **Issuer** - The entity that issued the token
- **Legal documents** - Links to operating agreements, etc.
- **Asset type** - Share, unit, etc.

## For Investors

### Receiving Tokens

When you invest in an offering or receive tokens:

1. **New lot created** automatically with:
   - Acquisition date = transaction date
   - Cost basis = price you paid
   - Payment currency = what you used (USDC, etc.)
   - Quantity = number of tokens received

2. **Tokens appear in your wallet**
3. **View lot details** in the Tokens section

### Viewing Your Tokens

Navigate to **Tokens** to see:

- **All tokens you hold**
- **Balance per token**
- **All lots** for each token
- **Cost basis** and acquisition date per lot
- **Current value** (if available)

### Transferring Tokens

To transfer tokens (if allowed by compliance rules):

1. Navigate to the token
2. Click **Transfer**
3. Choose which **lot** to transfer from
4. Enter **recipient address** and **quantity**
5. **Authenticate** with Face ID / Touch ID
6. Transaction submitted

**Note:** Transfers may be restricted by compliance conditions (lockups, vesting, etc.).

## For Issuers

### Creating Tokens

Only entity accounts can create tokens.

**Steps:**

1. **Switch to entity context** if not already
2. Navigate to **Tokens** → **Create Token**
3. **Select token type** (Share, Unit, etc.)
4. **Configure details** (name, symbol, decimals)
5. **Choose compliance preset** (lockups, vesting, etc.)
6. **Review and deploy**

**Time required:** 2-3 minutes

Learn more: [Creating a Token](creating-a-token.md)

### Issuing Tokens

After creating a token, issue (mint) tokens to investors:

1. Navigate to your token
2. Click **Issue Tokens**
3. Enter **recipient address**, **quantity**, **cost basis**, **payment currency**
4. Authenticate and submit

Learn more: [Issuing Tokens](issuing-tokens.md)

### Managing Your Tokens

As an issuer, you can:

- **Issue new tokens** - Mint tokens to investors
- **View all holders** - See who holds your tokens
- **View all lots** - See all lots across all holders
- **Pause transfers** - Temporarily disable transfers
- **Freeze accounts** - Prevent specific accounts from transferring
- **Update compliance** - Add or modify transfer restrictions

## Token Types

### Shares

Represents equity ownership in a corporation:

- **Common Stock** - Standard equity with voting rights
- **Preferred Stock** - Priority liquidation preference
- **Stock Options** - Right to purchase shares

### Units

Represents membership interest in an LLC or partnership:

- **Membership Units** - LLC membership interest
- **Partnership Interest** - Limited partnership interest
- **Profits Interest** - Equity compensation in partnerships

### Other Securities

- **Convertible Notes** - Debt that converts to equity
- **Warrants** - Right to purchase shares
- **SAFEs** - Simple Agreement for Future Equity

## Compliance Presets

When creating a token, choose a compliance preset:

### No Restrictions

Tokens are freely transferable with no lockups or restrictions.

**Use for:**
- Internal bookkeeping
- Non-securities
- Fully vested, unrestricted shares

### Lockup Period

Tokens cannot be transferred until a specific date.

**Use for:**
- IPO lockups
- Founder shares
- Employee grants with cliff

### Vesting Schedule

Tokens unlock gradually over time.

**Use for:**
- Employee compensation
- Advisor grants
- Founder vesting

### Rule 144

SEC Rule 144 holding period requirements.

**Use for:**
- Restricted securities
- Unregistered shares
- Reg D offerings

### Right of First Refusal (ROFR)

Company or other shareholders get first right to purchase before external transfer.

**Use for:**
- Private company shares
- Closely held corporations
- Partnership interests

## Technical Details

### ERC-7752 Standard

CapSign implements ERC-7752 with extensions:

```solidity
interface IToken {
  // Create a new lot (issuing/minting)
  function createLot(
    address owner,
    uint256 quantity,
    address paymentCurrency,
    uint256 costBasis,
    uint256 acquisitionDate,
    string memory uri,
    bytes memory data,
    uint256 customId
  ) external returns (bytes32 lotId, uint256 assignedCustomId);

  // Transfer a lot (selling/gifting)
  function transfer(
    bytes32 lotId,
    address to,
    uint256 quantity,
    TransferType tType,
    uint256 newCostBasis,
    string memory uri,
    bytes memory data
  ) external returns (bytes32 newLotId);

  // Get lot information
  function getLot(bytes32 lotId) external view returns (...);
}
```

Learn more: [Token Architecture](/protocol/tokens.md)

### Diamond Architecture

Tokens use the Diamond Pattern (EIP-2535):

- **TokenBalancesFacet** - ERC-20 compatibility
- **TokenLotsFacet** - Lot management (ERC-7752)
- **TokenTransferFacet** - Transfer logic
- **TokenComplianceFacet** - Compliance checks
- **TokenMetadataFacet** - Token information
- **TokenAdminFacet** - Administrative functions

This allows for:

- **Modularity** - Add/remove features without redeploying
- **Upgradeability** - Fix bugs and add features
- **Gas efficiency** - Singleton pattern reduces costs

### Deployed Contracts

See [Contract Addresses](/reference/contract-addresses.md) for token factory and facet addresses.

## FAQs

**Q: What's the difference between lots and tokens?**
A: Tokens are the asset (like "Class A shares"). Lots are groups of those tokens with the same acquisition date and cost basis.

**Q: Can I merge lots?**
A: No. Lots must remain separate for tax compliance. If you buy tokens at different times/prices, they stay in separate lots.

**Q: What if I want to sell tokens from a specific lot?**
A: When transferring, you choose which lot to transfer from. This allows tax optimization (FIFO, LIFO, or specific ID).

**Q: Are CapSign tokens ERC-20 compatible?**
A: Yes! Tokens implement ERC-20 for compatibility with wallets and exchanges. The lot data is an extension on top of ERC-20.

**Q: Can I create tokens without compliance restrictions?**
A: Yes, choose the "No Restrictions" preset. However, make sure you're complying with securities laws.

**Q: What happens to cost basis when I transfer tokens?**
A: The recipient's new lot can have a different cost basis (e.g., $0 for a gift, or sale price for a purchase).

## Guides

- [Creating a Token](creating-a-token.md) - Step-by-step token creation
- [Issuing Tokens](issuing-tokens.md) - How to mint tokens to investors
- [Lot-Based Accounting](lot-based-accounting.md) - Understanding lots and cost basis

## Need Help?

- **Email:** support@capsign.com
- **Twitter:** [@CapSignInc](https://twitter.com/CapSignInc)

