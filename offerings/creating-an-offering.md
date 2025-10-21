# Creating an Offering

This guide walks you through creating an investment offering on CapSign.

## Prerequisites

- Entity account with KYC completed
- Token already created
- Connected wallet (MetaMask/Coinbase Wallet)
- ETH for gas fees

## Step-by-Step Guide

### Step 1: Navigate to Create Offering

1. Log in and switch to entity context
2. Navigate to **Offerings** → **Create Offering**

### Step 2: Select Token

Choose which token you want to offer for sale.

- Only tokens you've created appear in the list
- If you don't see your token, create it first

### Step 3: Configure Offering

**Price Per Token** (in USDC)
- What investors pay per token
- Example: `1.00` = $1.00 per token

**Maximum Raise** (in USDC)
- Total amount you want to raise
- Example: `100000` = $100,000 maximum

**Minimum Investment** (in USDC)
- Smallest investment allowed
- Example: `1000` = $1,000 minimum per investor

### Step 4: Choose Compliance Preset

Select the securities exemption:

- **Reg D 506(b)** - Private placement, up to 35 non-accredited
- **Reg D 506(c)** - Accredited only, can advertise
- **Reg S** - Non-US investors
- **Reg A+** - Mini-IPO (up to $75M)
- **Reg D (General)** - General Reg D

### Step 5: Add Promotional Image (Optional)

Upload an image to make your offering more attractive:

- PNG, JPG, or WEBP
- Max 5MB
- Recommended: 1200x630px

### Step 6: Review and Deploy

Review all settings, then click **Deploy Offering**.

Sign the transaction in your wallet and wait for confirmation (~2-3 seconds).

## After Deployment

Your offering is now live! Next steps:

1. **Add documents** - Upload subscription agreement, PPM, etc.
2. **Share with investors** - If your exemption allows marketing
3. **Monitor investments** - Review and countersign investments
4. **Close offering** - When raise is complete

## See Also

- [Investing in Offerings](investing.md)
- [Compliance Options](compliance-options.md)
- [Offering Architecture](/protocol/offerings.md)
