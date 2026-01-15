# Factory Payment System Upgrade Guide

This guide walks through adding the `FactoryPaymentFacet` to existing TokenFactory and OfferingFactory diamonds via the interface UI.

## Overview

The payment facet enables factories to collect fees in multiple ERC20 tokens (USDC on Base mainnet, USDC + MockUSD on Base Sepolia) before creating tokens or offerings.

## Step 1: Deploy FactoryPaymentFacet

### Using Forge Script (Recommended)

```bash
cd protocol

# Deploy to Base Sepolia
forge script script/deploy/facets/DeployFactoryPaymentFacet.s.sol \
  --rpc-url https://sepolia.base.org \
  --broadcast \
  --verify

# Deploy to Base Mainnet (when ready)
forge script script/deploy/facets/DeployFactoryPaymentFacet.s.sol \
  --rpc-url https://mainnet.base.org \
  --broadcast \
  --verify
```

### Alternative: Using Cast

```bash
# Get the bytecode
BYTECODE=$(forge inspect FactoryPaymentFacet bytecode)

# Deploy to Base Sepolia
cast send --rpc-url https://sepolia.base.org \
  --private-key $PRIVATE_KEY \
  --create $BYTECODE
```

**Note the deployed address** - you'll need it for the diamond cut.

## Step 2: Prepare Diamond Cut Parameters

### Function Selectors

The payment facet has these functions:

```solidity
// Initialization
FactoryPayment_init(address feeRecipient)

// Payment Collection
collectPayment(address paymentToken, string deploymentType)

// Admin Functions
configurePaymentToken(address paymentToken, uint256 feeAmount, bool isActive)
setFeeRecipient(address feeRecipient)
setPaymentsEnabled(bool enabled)

// View Functions
getPaymentTokenConfig(address paymentToken) returns (bool, uint256, uint256)
getAcceptedTokens() returns (address[])
getFeeRecipient() returns (address)
getPaymentStats() returns (bool, uint256)
isPaymentInitialized() returns (bool)
```

### Generate Selectors

You can generate selectors using this command:

```bash
cd protocol

# Generate all selectors
cast sig "FactoryPayment_init(address)"
cast sig "collectPayment(address,string)"
cast sig "configurePaymentToken(address,uint256,bool)"
cast sig "setFeeRecipient(address)"
cast sig "setPaymentsEnabled(bool)"
cast sig "getPaymentTokenConfig(address)"
cast sig "getAcceptedTokens()"
cast sig "getFeeRecipient()"
cast sig "getPaymentStats()"
cast sig "isPaymentInitialized()"
```

Or use this helper script:

```bash
# Save to protocol/script/utils/get-payment-selectors.sh
#!/bin/bash
echo "FactoryPayment_init: $(cast sig 'FactoryPayment_init(address)')"
echo "collectPayment: $(cast sig 'collectPayment(address,string)')"
echo "configurePaymentToken: $(cast sig 'configurePaymentToken(address,uint256,bool)')"
echo "setFeeRecipient: $(cast sig 'setFeeRecipient(address)')"
echo "setPaymentsEnabled: $(cast sig 'setPaymentsEnabled(bool)')"
echo "getPaymentTokenConfig: $(cast sig 'getPaymentTokenConfig(address)')"
echo "getAcceptedTokens: $(cast sig 'getAcceptedTokens()')"
echo "getFeeRecipient: $(cast sig 'getFeeRecipient()')"
echo "getPaymentStats: $(cast sig 'getPaymentStats()')"
echo "isPaymentInitialized: $(cast sig 'isPaymentInitialized()')"

chmod +x script/utils/get-payment-selectors.sh
./script/utils/get-payment-selectors.sh
```

## Step 3: Execute Diamond Cut via Interface UI

### 3.1 Navigate to Factory Management

1. Go to your app: `https://app.capsign.local` (or production URL)
2. Connect your wallet (must be admin of the factory)
3. Navigate to **Admin** → **Factory Management**

### 3.2 Prepare Cut Data

**For TokenFactory:**

```json
{
  "factoryAddress": "0x[TokenFactory address]",
  "facetCuts": [
    {
      "facet": "0x[FactoryPaymentFacet deployed address]",
      "action": 0,
      "selectors": [
        "0x[FactoryPayment_init selector]",
        "0x[collectPayment selector]",
        "0x[configurePaymentToken selector]",
        "0x[setFeeRecipient selector]",
        "0x[setPaymentsEnabled selector]",
        "0x[getPaymentTokenConfig selector]",
        "0x[getAcceptedTokens selector]",
        "0x[getFeeRecipient selector]",
        "0x[getPaymentStats selector]",
        "0x[isPaymentInitialized selector]"
      ]
    }
  ],
  "init": "0x[FactoryPaymentFacet address]",
  "initData": "[encoded FactoryPayment_init call]"
}
```

**Encoding initData:**

```bash
# For Base Sepolia - initialize with your fee recipient address
cast abi-encode "FactoryPayment_init(address)" "0xYourFeeRecipientAddress"

# Example output: 0x1234abcd...
# Prepend the function selector
cast sig "FactoryPayment_init(address)" # Get selector: 0xabcd1234
# Final initData: 0xabcd1234[encoded parameters]
```

Or use this helper:

```bash
# Encode the full init call
cast calldata "FactoryPayment_init(address)" "0xYourFeeRecipientAddress"
```

### 3.3 UI Form Fields

The interface should have a form like this:

**Factory Diamond Cut Form:**

```
┌─────────────────────────────────────────────────┐
│ Factory Address                                  │
│ [0x...]                                         │
├─────────────────────────────────────────────────┤
│ Facet Address                                    │
│ [0x...FactoryPaymentFacet]                      │
├─────────────────────────────────────────────────┤
│ Action                                           │
│ [Add Facet ▼]                                   │
├─────────────────────────────────────────────────┤
│ Function Selectors (comma-separated)             │
│ [0xabcd1234, 0xefgh5678, ...]                   │
├─────────────────────────────────────────────────┤
│ Initialize After Cut?                            │
│ [✓] Yes  [ ] No                                 │
├─────────────────────────────────────────────────┤
│ Init Address (if initializing)                   │
│ [0x...FactoryPaymentFacet]                      │
├─────────────────────────────────────────────────┤
│ Init Function                                    │
│ [FactoryPayment_init ▼]                         │
├─────────────────────────────────────────────────┤
│ Fee Recipient                                    │
│ [0x...YourTreasuryAddress]                      │
├─────────────────────────────────────────────────┤
│                                                  │
│           [Preview Transaction]                  │
│           [Execute Diamond Cut]                  │
└─────────────────────────────────────────────────┘
```

### 3.4 Execute the Cut

1. Fill in all fields
2. Click **Preview Transaction** to review the calldata
3. Click **Execute Diamond Cut**
4. Approve the transaction in your wallet
5. Wait for confirmation

## Step 4: Configure Payment Tokens

After the diamond cut is complete, configure which tokens to accept:

### 4.1 Navigate to Payment Configuration

1. Go to **Admin** → **Factory Payment Config**
2. Select factory: **Token Factory** or **Offering Factory**

### 4.2 Add Payment Tokens

**For Base Sepolia:**

```
Token 1: USDC
├─ Address: 0x036CbD53842c5426634e7929541eC2318f3dCF7e
├─ Fee Amount: 1000000000 (1000 USDC - 6 decimals)
└─ Active: ✓

Token 2: Mock USD
├─ Address: 0xA1fd596484Eb5C4c69a3652Fff1CF6B8eE87eccD
├─ Fee Amount: 1000000000 (1000 mUSD - 6 decimals)
└─ Active: ✓
```

**For Base Mainnet:**

```
Token 1: USDC
├─ Address: 0x833589fcd6edb6e08f4c7c32d4f71b54bda02913
├─ Fee Amount: 1000000000 (1000 USDC - 6 decimals)
└─ Active: ✓
```

### 4.3 Execute Configuration

For each token, call:

```solidity
factoryDiamond.configurePaymentToken(
  tokenAddress,
  feeAmount,
  true // isActive
)
```

**UI Form:**

```
┌─────────────────────────────────────────────────┐
│ Configure Payment Token                          │
├─────────────────────────────────────────────────┤
│ Factory                                          │
│ [Token Factory ▼]                               │
├─────────────────────────────────────────────────┤
│ Token Address                                    │
│ [0x...USDC]                                     │
├─────────────────────────────────────────────────┤
│ Fee Amount (in token's smallest unit)            │
│ [1000000000]                                    │
│ (1000 tokens with 6 decimals)                   │
├─────────────────────────────────────────────────┤
│ Status                                           │
│ [✓] Active  [ ] Inactive                        │
├─────────────────────────────────────────────────┤
│                                                  │
│           [Configure Token]                      │
└─────────────────────────────────────────────────┘
```

## Step 5: Verify Installation

### Check via UI

Navigate to **Admin** → **Factory Info** and verify:

- ✓ Payment facet is listed in installed facets
- ✓ Payment recipient is set correctly
- ✓ Accepted tokens are configured
- ✓ Fee amounts are correct

### Check via Cast

```bash
# Check if initialized
cast call $FACTORY_ADDRESS "isPaymentInitialized()(bool)" \
  --rpc-url https://sepolia.base.org

# Get fee recipient
cast call $FACTORY_ADDRESS "getFeeRecipient()(address)" \
  --rpc-url https://sepolia.base.org

# Get accepted tokens
cast call $FACTORY_ADDRESS "getAcceptedTokens()(address[])" \
  --rpc-url https://sepolia.base.org

# Get token config (USDC example)
cast call $FACTORY_ADDRESS \
  "getPaymentTokenConfig(address)(bool,uint256,uint256)" \
  "0x036CbD53842c5426634e7929541eC2318f3dCF7e" \
  --rpc-url https://sepolia.base.org
```

## Step 6: Test Payment Flow

### 6.1 Get Test Tokens

**On Base Sepolia:**

```bash
# Get MockUSD from faucet
cast send 0xA1fd596484Eb5C4c69a3652Fff1CF6B8eE87eccD \
  "faucet()" \
  --rpc-url https://sepolia.base.org \
  --private-key $PRIVATE_KEY
```

### 6.2 Test Token Creation with Payment

Via UI:
1. Go to **Tokens** → **Create Token**
2. Fill in token details
3. Select payment token: **MockUSD** or **USDC**
4. Approve token spend (1000 tokens)
5. Create token

The UI should:
- Show payment token selection dropdown
- Display fee amount: "Fee: 1000 mUSD" or "Fee: 1000 USDC"
- Request token approval before creation
- Collect payment automatically during creation

## Repeat for OfferingFactory

Follow the same steps for the **OfferingFactory** diamond:

1. Execute diamond cut with payment facet
2. Initialize with fee recipient
3. Configure payment tokens
4. Test offering creation with payment

## Troubleshooting

### "Function selector collision"
- Check that selectors aren't already in the diamond
- Use `cast call $FACTORY "facetFunctionSelectors(address)"` to check existing selectors

### "Initialization failed"
- Verify fee recipient address is not zero address
- Check that init address matches facet address
- Verify initData is correctly encoded

### "Payment token not accepted"
- Ensure token is configured via `configurePaymentToken()`
- Check token is active: `getPaymentTokenConfig(token)`

### "Insufficient allowance"
- User must approve factory to spend payment token
- Amount must be >= fee amount
- Check allowance: `cast call $TOKEN "allowance(address,address)" $USER $FACTORY`

## Summary Checklist

- [ ] Deploy FactoryPaymentFacet to network
- [ ] Execute diamond cut on TokenFactory
- [ ] Initialize TokenFactory payment module
- [ ] Configure USDC for TokenFactory
- [ ] Configure MockUSD for TokenFactory (testnet only)
- [ ] Execute diamond cut on OfferingFactory
- [ ] Initialize OfferingFactory payment module
- [ ] Configure USDC for OfferingFactory
- [ ] Configure MockUSD for OfferingFactory (testnet only)
- [ ] Test token creation with payment
- [ ] Test offering creation with payment
- [ ] Update frontend to show payment options
- [ ] Verify fee collection in treasury

## Next Steps

After successful deployment:
1. Monitor payment events for revenue tracking
2. Implement invoice generation based on `PaymentCollected` events
3. Add admin UI for fee adjustments
4. Consider dynamic pricing based on token type or features

