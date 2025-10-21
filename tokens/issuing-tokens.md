# Issuing Tokens

After creating a token, you need to issue (mint) tokens to investors. This guide explains how.

## What is Issuing?

**Issuing** (or **minting**) creates new tokens and assigns them to an address. This is how you:

- Give shares to founders
- Issue equity to employees
- Distribute tokens to investors after they fund an offering
- Award tokens for any other reason

## Prerequisites

- **Token created** - You must have created a token first
- **Entity context** - You must be in an entity context
- **Recipient address** - Know who you're issuing to
- **Cost basis info** - Know the price, currency, and date

## How Issuing Works

When you issue tokens:

1. **New lot created** with:
   - Recipient address
   - Quantity of tokens
   - Cost basis per token
   - Payment currency
   - Acquisition date
   - Transfer type

2. **Tokens minted** to recipient's wallet
3. **Total supply increases** by the quantity issued
4. **Lot appears** in recipient's wallet with full tracking

## Step-by-Step Guide

### Step 1: Navigate to Your Token

1. **Log in** and switch to entity context
2. Navigate to **Tokens**
3. **Click on the token** you want to issue

### Step 2: Click "Issue Tokens"

Click the **Issue Tokens** button.

### Step 3: Enter Recipient Information

**Recipient Address**

Enter the wallet address receiving the tokens.

Example: `0x1234567890123456789012345678901234567890`

**Tips:**
- Double-check the address - transactions can't be reversed
- You can paste from clipboard
- Recipient doesn't need to be KYC'd yet (compliance checked on transfer)

**Quantity**

Enter the number of tokens to issue.

Examples:
- `1000` - 1,000 tokens (if decimals = 0)
- `1000.5` - 1,000.5 tokens (if decimals > 0)

**Notes:**
- Must be a positive number
- Decimals must match token configuration
- No maximum (you can issue unlimited supply)

### Step 4: Enter Cost Basis Information

**Cost Basis Per Token**

The price per token the recipient paid (or $0 if a gift).

Examples:
- `1.00` - $1.00 per token
- `0.50` - $0.50 per token
- `0` - Free (gift, grant, etc.)

**Payment Currency**

What currency was used for payment:

- **USDC** - Most common for investments
- **ETH** - If paid in ETH
- **USD** - For fiat payments (off-chain)
- **None** - For gifts/grants

**Acquisition Date**

When the recipient acquired these tokens.

Options:
- **Today** (default)
- **Custom date** - For backdating (e.g., vesting start date)

**Transfer Type**

How the recipient acquired the tokens:

- **Purchase** - Bought in an offering or secondary sale
- **Gift** - Received as a gift (cost basis = $0)
- **Grant** - Employee stock grant
- **Exercise** - Option exercise
- **Other** - Custom reason

### Step 5: Review and Submit

Review the issuance details:

```
Recipient: 0x1234...5678
Quantity: 1,000 tokens
Cost Basis: $1.00 per token
Currency: USDC
Date: 2024-01-15
Type: Purchase
```

Click **Issue Tokens**.

### Step 6: Sign Transaction

1. **Transaction prompt** appears
2. **Review in wallet** (MetaMask/Coinbase Wallet)
3. **Sign transaction**
4. **Wait for confirmation** (2-3 seconds)

**Gas cost:** Typically $0.01 - $0.10 on Base.

## After Issuing

### What Happens

1. **Tokens appear** in recipient's wallet immediately
2. **New lot created** with all cost basis info
3. **Total supply** increases
4. **Holder count** increases (if new holder)
5. **Transaction recorded** on-chain

### Recipient View

The recipient will see:

- New token balance
- New lot with:
  - Quantity
  - Acquisition date
  - Cost basis
  - Payment currency
  - Transfer type

## Common Scenarios

### Scenario 1: Founder Shares

**Context:** Issuing shares to founders at formation.

**Settings:**
- Cost Basis: `0` (or nominal amount like `0.001`)
- Currency: `None` or `USD`
- Date: Formation date
- Type: `Grant`

### Scenario 2: Employee Stock Grant

**Context:** Granting equity to an employee.

**Settings:**
- Cost Basis: `0`
- Currency: `None`
- Date: Grant date
- Type: `Grant`

**Note:** Add vesting compliance to the token to enforce vesting schedule.

### Scenario 3: Investor Purchase

**Context:** Investor bought tokens in an offering.

**Settings:**
- Cost Basis: Price per token (e.g., `1.00`)
- Currency: `USDC`
- Date: Investment date
- Type: `Purchase`

### Scenario 4: Option Exercise

**Context:** Employee exercised stock options.

**Settings:**
- Cost Basis: Strike price
- Currency: `USDC` or `USD`
- Date: Exercise date
- Type: `Exercise`

### Scenario 5: Secondary Sale

**Context:** Buying tokens from existing holder.

**Settings:**
- Cost Basis: Purchase price
- Currency: `USDC`
- Date: Purchase date
- Type: `Purchase`

## Batch Issuing

Need to issue to multiple recipients?

### Option 1: Manual (Small Batches)

Issue tokens one at a time through the UI.

**Good for:** < 10 recipients

### Option 2: Contact Support (Large Batches)

**For:** > 10 recipients

Contact support@capsign.com with:
- Token address
- CSV file with recipient addresses, quantities, cost basis

We can help with bulk issuance.

### Option 3: Smart Contract (Advanced)

Call `createLot` directly on the token contract.

**For:** Developers comfortable with smart contracts

See [Token Architecture](/protocol/tokens.md) for technical details.

## Compliance Considerations

### Transfer Restrictions

If your token has compliance rules (lockup, vesting, etc.):

- Restrictions apply **immediately** after issuance
- Recipients can't transfer until restrictions lift
- Lot metadata includes restriction info

### Accreditation

If your token requires accredited investors:

- Issuance itself doesn't check accreditation
- Accreditation checked on **transfer**
- Recipient must complete KYC + accreditation attestation to transfer

### Securities Laws

Issuing tokens = issuing securities. Ensure you:

- Have proper legal documents (subscription agreement, etc.)
- Comply with Reg D, Reg S, Reg A+, or other exemptions
- File required paperwork (Form D, etc.)
- Maintain investor records

**Consult legal counsel** before issuing securities.

## Troubleshooting

### "Only admins can issue tokens"

**Problem:** You don't have admin role on the token.

**Solution:** Ensure you're the entity that created the token or have been granted admin role.

### "Insufficient gas"

**Problem:** Not enough ETH for transaction fees.

**Solution:** Send ETH to your entity wallet.

### "Invalid address"

**Problem:** Recipient address is malformed.

**Solutions:**
- Ensure it's a valid Ethereum address (0x...)
- Check for typos
- Try copying again from source

### "Token paused"

**Problem:** Token has been paused by admin.

**Solution:** Unpause the token in token settings before issuing.

## Best Practices

### Record Keeping

- **Document issuances** - Keep records matching on-chain data
- **Update cap table** - Reflect issuances in your internal cap table
- **Legal agreements** - Have signed subscription agreements before issuing
- **Audit trail** - Save transaction hashes for all issuances

### Cost Basis Accuracy

- **Use correct values** - Accurate cost basis is critical for taxes
- **Match legal docs** - Cost basis should match subscription agreement
- **Currency consistency** - Use the same currency for all issuances in a round
- **Date accuracy** - Use the actual investment/grant date

### Security

- **Verify addresses** - Always double-check recipient addresses
- **Test first** - Issue a small amount first if uncertain
- **Use checksummed addresses** - Prevents some typos
- **Confirm receipt** - Ask recipient to confirm they received tokens

### Communication

- **Notify recipients** - Let them know tokens have been issued
- **Provide wallet address** - Share your token's contract address
- **Explain lots** - Help recipients understand lot-based accounting
- **Set expectations** - Explain any transfer restrictions

## Advanced Topics

### Custom IDs

You can assign custom IDs to lots for easier tracking:

```solidity
createLot(
  owner,
  quantity,
  paymentCurrency,
  costBasis,
  acquisitionDate,
  uri,
  data,
  customId  // e.g., 1, 2, 3...
)
```

Custom IDs are useful for matching lots to internal records.

### Lot Metadata

Store additional data in the lot:

- Subscription agreement hash
- Investor ID
- Series/round information
- Custom notes

### Adjusting Lots

Made a mistake? You can adjust a lot:

1. Navigate to the lot
2. Click **Adjust**
3. Enter new quantity or cost basis
4. Old lot is invalidated
5. New lot created with corrected info

**Note:** This creates an audit trail - the old lot remains visible as "invalidated."

## FAQs

**Q: Can I issue tokens to myself?**
A: Yes, but be mindful of tax implications. Consult your accountant.

**Q: What if I issue to the wrong address?**
A: Contact the recipient and request they transfer back. Issuances can't be reversed.

**Q: Can I issue more tokens after initial issuance?**
A: Yes, you can issue additional tokens anytime (no maximum supply).

**Q: Do I need to issue all tokens at once?**
A: No, issue as needed (after investments, employee grants, etc.).

**Q: Can different lots have different cost basis?**
A: Yes! Each issuance creates a new lot with its own cost basis.

**Q: What if the recipient doesn't have a CapSign account?**
A: They can receive tokens at any Ethereum address. They'll need to create an account to view lot details.

## Next Steps

After issuing tokens:

- **[Create an Offering](../offerings/creating-an-offering.md)** - If raising capital
- **[Manage Compliance](lot-based-accounting.md)** - Learn about lot tracking
- **View Token Holders** - See all holders and balances

## Need Help?

- **Email:** support@capsign.com
- **Twitter:** [@CapSignInc](https://twitter.com/CapSignInc)

