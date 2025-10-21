# Lot-Based Accounting

CapSign uses **lot-based accounting** (ERC-7752) to track every token's acquisition date, cost basis, and origin - essential for tax reporting and regulatory compliance.

## What is a Lot?

A **lot** is a group of tokens with the same:

- **Acquisition date** - When you got them
- **Cost basis** - What you paid per token
- **Payment currency** - What currency you used
- **Transfer type** - How you acquired them

## Why Lots Matter

### Tax Reporting

When you sell tokens, you need to report:

- **Capital gain/loss** = Sale price - Cost basis
- **Holding period** = Sale date - Acquisition date
- **Long-term vs short-term** = > 1 year or < 1 year

**Lots provide all this data automatically.**

### Tax Optimization

With multiple lots, you can choose which to sell:

- **FIFO** - First In, First Out (sell oldest first)
- **LIFO** - Last In, First Out (sell newest first)
- **Specific ID** - Choose exact lots to sell
- **Highest cost** - Sell highest cost basis first (minimize gains)

### Regulatory Compliance

Securities regulations require:

- **Accurate record keeping** - Know when each share was acquired
- **Holding period tracking** - For Rule 144, Reg D restrictions
- **Cost basis reporting** - For IRS Form 8949

**Lots make compliance automatic.**

## How Lots Work

### Lot Creation

A new lot is created when you:

1. **Receive tokens** from an offering
2. **Are issued tokens** by an issuer
3. **Purchase tokens** in a secondary sale
4. **Receive a gift** of tokens

Each event creates a **separate lot** with its own metadata.

### Example

You buy Class A shares three times:

**January 1, 2024:**
- Buy 100 shares @ $1.00 each
- Lot 1: 100 shares, $1.00 cost basis, Jan 1 2024

**March 1, 2024:**
- Buy 50 shares @ $1.50 each
- Lot 2: 50 shares, $1.50 cost basis, Mar 1 2024

**June 1, 2024:**
- Buy 200 shares @ $0.75 each
- Lot 3: 200 shares, $0.75 cost basis, Jun 1 2024

**Total:** 350 shares across 3 lots.

### Viewing Lots

In CapSign, navigate to a token to see:

- **Total balance** - All lots combined (350 shares)
- **Individual lots** - List of each lot with details
- **Cost basis** - Per-lot and weighted average
- **Acquisition dates** - For holding period calculation

## Transfer Types

Each lot has a **transfer type** indicating how you acquired it:

### Purchase

You bought the tokens with money.

**Cost basis:** Price paid
**Tax treatment:** Capital asset

### Gift

You received tokens as a gift.

**Cost basis:** Donor's cost basis (carried over)
**Tax treatment:** Special gift rules apply

### Grant

You received tokens as employee compensation.

**Cost basis:** $0 (typically)
**Tax treatment:** Ordinary income at vest

### Exercise

You exercised stock options.

**Cost basis:** Strike price paid
**Tax treatment:** Depends on option type (ISO vs NSO)

### Other

Custom transfer type for special situations.

## Selling Tokens

### Choosing Lots to Sell

When you transfer/sell tokens, you choose which lot:

1. Navigate to token
2. Click **Transfer**
3. **Select lot** to transfer from
4. Enter quantity and recipient
5. Confirm transaction

The app shows each lot with:
- Quantity available
- Cost basis
- Acquisition date
- Days held

### Tax Optimization Strategies

#### FIFO (First In, First Out)

Sell oldest lots first.

**Good for:**
- Long-term capital gains (lower rate)
- Simple accounting

**Example:** Sell Lot 1 first (Jan 1 purchase).

#### LIFO (Last In, First Out)

Sell newest lots first.

**Good for:**
- Minimizing gains in up market
- Harvesting short-term losses

**Example:** Sell Lot 3 first (Jun 1 purchase).

#### Highest Cost First

Sell lots with highest cost basis first.

**Good for:**
- Minimizing capital gains
- Loss harvesting

**Example:** Sell Lot 2 first ($1.50 cost basis).

#### Specific Identification

Choose exact lots based on your situation.

**Good for:**
- Complex tax planning
- Optimizing for specific goals

**Requires:** Careful record keeping (CapSign handles this automatically).

### New Lot for Recipient

When you transfer tokens:

1. **Your lot** - Quantity reduced
2. **Recipient's new lot** - Created with:
   - Their acquisition date (transfer date)
   - Their cost basis (negotiated price)
   - Their payment currency
   - Transfer type (usually "Purchase")

**This means:** Recipient's lot is independent of yours.

## Reporting for Taxes

### Form 8949

When you sell tokens, report on IRS Form 8949:

| Description | Date Acquired | Date Sold | Proceeds | Cost Basis | Gain/Loss |
|-------------|---------------|-----------|----------|------------|-----------|
| 100 CSA | 01/01/2024 | 12/01/2024 | $150 | $100 | $50 |

**CapSign provides all this data.**

### Export Lot Data

Export your lot history for your accountant:

1. Navigate to token
2. Click **Export**
3. Choose format (CSV, PDF)
4. Send to accountant

## Lot Operations

### Splitting Lots

When you transfer part of a lot:

**Before:**
- Lot 1: 100 shares @ $1.00

**Transfer 30 shares:**
- Your Lot 1: 70 shares @ $1.00 (remaining)
- Recipient's new lot: 30 shares @ $2.00 (if sold for $2.00)

**Note:** Your cost basis stays $1.00 on remaining shares.

### Merging Lots

**You cannot merge lots.** Each lot must stay separate for tax compliance.

**Example:** If you buy twice at different prices, you'll always have 2 lots (even same date).

### Adjusting Lots

Made a mistake in cost basis?

1. Navigate to lot
2. Click **Adjust**
3. Enter correct cost basis
4. Old lot invalidated
5. New lot created

**Note:** Creates audit trail - old lot visible as "invalidated."

## Advanced Topics

### Gifting Tokens

When you gift tokens:

**Your side:**
- Lot quantity reduced
- No taxable event (generally)

**Recipient side:**
- New lot created
- **Cost basis = your cost basis** (carried over)
- **Acquisition date = your date** (for long-term holding)
- Transfer type = "Gift"

**Important:** Consult tax advisor for gift tax implications.

### Employee Grants

Employee stock grants with vesting:

**At grant:**
- Lot created with 0 cost basis
- Vesting schedule attached

**At vest:**
- Ordinary income = FMV at vest
- Cost basis = FMV at vest (for future sales)

**At sale:**
- Capital gain/loss = Sale price - Cost basis
- Holding period starts at vest

### Complex Scenarios

#### Wash Sales

If you sell at a loss and rebuy within 30 days:

- Wash sale rule applies
- Cost basis adjusted
- CapSign doesn't automatically detect this - consult accountant

#### Like-Kind Exchanges

Exchanging one security for another:

- May defer gains
- Lots carry over
- Consult tax advisor

## FAQs

**Q: Why can't I merge lots?**
A: Tax regulations require separate lots for different acquisition dates/prices.

**Q: What if I don't know my cost basis?**
A: You must track it. Without cost basis, IRS assumes $0, maximizing your tax.

**Q: Can I delete a lot?**
A: No. Lots are permanent for audit trail. You can invalidate and create corrected lot.

**Q: What if I received tokens before CapSign?**
A: Migrate to CapSign by issuing with historical cost basis and acquisition date.

**Q: Do I need lots if I never plan to sell?**
A: Yes. Required for estate planning, gifting, and compliance.

**Q: How accurate is the cost basis data?**
A: As accurate as the data entered at issuance. Verify with your records.

## Need Help?

- **Tax Questions:** Consult a tax professional
- **Technical Support:** support@capsign.com
- **Twitter:** [@CapSignInc](https://twitter.com/CapSignInc)

