# Creating a Token

This guide walks you through creating a security token on CapSign.

## Prerequisites

### Requirements

- **Entity account** - Only entities can create tokens (not individual accounts)
- **KYC completed** - Your entity must have completed identity verification
- **Connected wallet** - MetaMask or Coinbase Wallet connected (for entity contexts)
- **ETH for gas** - Small amount of ETH on Base for transaction fees

### Before You Start

Have ready:

- **Token name** - Full name (e.g., "Class A Common Stock")
- **Token symbol** - 2-10 character ticker (e.g., "CSA")
- **Decimals** - Usually 0 for shares/units, 18 for divisible tokens
- **Transfer restrictions** - What compliance rules you need

## Step-by-Step Guide

### Step 1: Navigate to Create Token

1. **Log in** to CapSign
2. **Switch to entity context** (if not already)
3. Navigate to **Tokens** → **Create Token**

### Step 2: Select Token Type

Choose the type of security you're creating:

**Shares** - Equity in a corporation
- Common Stock
- Preferred Stock

**Units** - Membership interest in an LLC/partnership
- Membership Units
- Partnership Interest

**Note:** Your available options depend on your entity's legal form.

### Step 3: Configure Token Details

Enter the token information:

#### Token Name

The full, legal name of the security.

**Examples:**
- "Class A Common Stock"
- "Series Seed Preferred Stock"
- "Membership Units"

**Best practices:**
- Use the exact name from your legal documents
- Include class/series if applicable
- Don't include your company name (it's added automatically)

#### Token Symbol

A 2-10 character ticker symbol.

**Standard prefixes:**
- `CS` - Common Stock (e.g., CSA, CSB)
- `PS` - Preferred Stock (e.g., PSA, PSB, PSS for Seed)
- `MU` - Membership Units

**Examples:**
- Class A Common Stock → `CSA`
- Series A Preferred Stock → `PSA`
- Series Seed Preferred Stock → `PSS`
- Membership Units → `MU`

**Best practices:**
- Keep it short and memorable
- Use standard prefixes when applicable
- Don't include your company name

#### Decimals

How divisible the token is.

**Common choices:**
- **0 decimals** - Whole units only (most common for shares/units)
- **18 decimals** - Fully divisible (like ETH)
- **6 decimals** - Less divisible (like USDC)

**Recommendation:** Use **0 decimals** for shares and units unless you have a specific reason to make them divisible.

### Step 4: Choose Compliance Preset

Select transfer restrictions for your token:

#### No Restrictions

Tokens are freely transferable with no limits.

**Use for:**
- Internal bookkeeping
- Fully vested shares
- Non-securities

**Features:**
- ✅ Immediate transferability
- ✅ No lockups
- ✅ No accreditation requirements

#### Lockup Period

Tokens cannot be transferred until a specific date.

**Use for:**
- IPO lockups
- Founder shares
- Employee grants with cliff

**Features:**
- 🔒 Cannot transfer until lockup expires
- ⏰ Lockup date configurable per lot
- ✅ Automatic unlock on date

#### Vesting Schedule

Tokens unlock gradually over time.

**Use for:**
- Employee stock grants
- Advisor equity
- Founder vesting

**Features:**
- ⏰ Linear vesting over period
- 🔒 Cliff period (optional)
- 📊 Partial vesting allowed

#### Rule 144

SEC Rule 144 holding period requirements (typically 6-12 months).

**Use for:**
- Restricted securities under Reg D
- Unregistered shares
- Securities sold to accredited investors

**Features:**
- 🕐 6-month minimum holding period
- 📝 Automatic tracking
- ✅ Compliance with SEC rules

#### Right of First Refusal (ROFR)

Company or existing shareholders get first right to purchase before external transfers.

**Use for:**
- Private company shares
- Closely held corporations
- LP interests in funds

**Features:**
- 🚫 External transfers require approval
- ⏱️ ROFR period (typically 30-90 days)
- ✅ Internal transfers unrestricted

### Step 5: Review Compliance Notice

Read and acknowledge the legal disclaimer:

> You are responsible for ensuring compliance with all applicable securities laws. CapSign provides technology tools but does not provide legal advice. Consult with qualified legal counsel before issuing securities.

Check the acknowledgment box to proceed.

### Step 6: Review Configuration

Review all details:

- **Token Type**: Share
- **Name**: Class A Common Stock
- **Symbol**: CSA
- **Decimals**: 0
- **Compliance**: Rule 144 (6-month holding period)

Double-check everything - tokens cannot be deleted after deployment.

### Step 7: Deploy Token

1. Click **Deploy Token**
2. **Sign the transaction** with your connected wallet (MetaMask/Coinbase Wallet)
3. **Wait for confirmation** (typically 2-3 seconds on Base)
4. **Token deployed!**

**Gas costs:** Typically $0.01 - $0.10 on Base.

## After Deployment

### What Happens Next

1. **Token appears** in your Tokens list
2. **Initial supply is 0** - no tokens issued yet
3. **You can now issue tokens** to investors

### Next Steps

- **[Issue Tokens](issuing-tokens.md)** - Mint tokens to investors
- **View token details** - See contract address, total supply, holders
- **Create an offering** - If you want to raise capital

## Naming Conventions

### Shares (Corporations)

**Common Stock:**
- Class A Common Stock → `CSA`
- Class B Common Stock → `CSB`
- Common Stock → `CS`

**Preferred Stock:**
- Series A Preferred Stock → `PSA`
- Series B Preferred Stock → `PSB`
- Series Seed Preferred Stock → `PSS`

### Units (LLCs/Partnerships)

**LLC:**
- Class A Membership Units → `MUA`
- Membership Units → `MU`

**Partnerships:**
- Limited Partnership Interest → `LPI`
- General Partnership Interest → `GPI`

## Troubleshooting

### "Only entities can create tokens"

**Problem:** You're in an individual account context.

**Solution:** Switch to an entity context in the account menu.

### "KYC verification required"

**Problem:** Your entity hasn't completed KYC.

**Solution:** Complete entity KYC in Settings → Profile.

### "Insufficient funds"

**Problem:** Not enough ETH for gas.

**Solution:** Send ETH to your entity wallet address.

### "Transaction failed"

**Problem:** Various reasons (gas, network, etc.)

**Solutions:**
- Check that you have enough ETH for gas
- Try again (network may be congested)
- Refresh the page and retry
- Contact support if issue persists

## Best Practices

### Legal Compliance

- **Consult legal counsel** before creating securities
- **Ensure proper formation** (corporation, LLC, etc.)
- **Have operating agreements** or bylaws in place
- **Understand securities laws** in your jurisdiction

### Token Design

- **Use 0 decimals** for traditional shares/units
- **Choose appropriate restrictions** based on your legal agreements
- **Follow naming conventions** for clarity
- **Keep symbols short** (2-4 characters ideal)

### After Creation

- **Document the token** - Save contract address, transaction hash
- **Update cap table** - Reflect on-chain token in your records
- **Inform stakeholders** - Let shareholders know about the token
- **Issue carefully** - Double-check recipient addresses before issuing

## Advanced Features

### Custom Compliance

Need custom transfer restrictions?

**Contact us** to implement:
- Investor limits (e.g., max 99 investors for Reg D)
- Transfer approval workflows
- Jurisdiction restrictions
- Custom business logic

### Multiple Token Classes

You can create multiple tokens for different share classes:

- Class A Common Stock (CSA)
- Class B Common Stock (CSB)
- Series Seed Preferred Stock (PSS)
- Series A Preferred Stock (PSA)

Each token is independent with its own compliance rules.

## FAQs

**Q: Can I change the token name or symbol after deployment?**
A: No. Token details are immutable. Create a new token if you need different details.

**Q: Can I add or change compliance rules after deployment?**
A: Yes. You can add new compliance conditions to an existing token.

**Q: How much does it cost to create a token?**
A: Gas fees only, typically $0.01 - $0.10 on Base.

**Q: Can I delete a token?**
A: No. Once deployed, tokens are permanent on the blockchain.

**Q: What if I make a mistake?**
A: If you haven't issued any tokens yet, you can just create a new token with correct details. The unused token remains but doesn't affect anything.

**Q: Do I need to create a new token for each round of fundraising?**
A: No. One token can be used across multiple offerings (unless you're creating a new share class).

## Need Help?

- **Email:** support@capsign.com
- **Twitter:** [@CapSignInc](https://twitter.com/CapSignInc)
- **Legal Questions:** Consult your attorney

