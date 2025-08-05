# CMX Token Utility & Economics

The CMX token is the native asset of the CMX Network and provides real utility across the CapSign ecosystem, including **significant discounts** on infrastructure services.

## Core Token Utilities

### **1. 🏗️ Infrastructure Service Payments**

- **20% discount** on all CapSign services when paying with CMX
- **Volume discounts** up to 40% off USD pricing for large deployments
- **Predictable pricing** with 7-day price locks for budgeting

### **2. ⛽ CMX Network Gas Fees**

- **Native gas token** for all CMX Network transactions
- **Lower fees** compared to Ethereum mainnet
- **Fast finality** with Optimism's 2-second block times

### **3. 🏛️ Network Governance**

- **Vote on network upgrades** and parameter changes
- **Propose improvements** to the CMX Network
- **Participate in CapSign DAO** governance decisions

### **4. 🔒 Protocol Staking**

- **Stake CMX** to secure the CapSign Protocol
- **Earn rewards** from protocol fees and network usage
- **Validator requirements** for CMX Network consensus

### **5. 💰 Fee Value Accrual**

- **Protocol fees** collected in CMX tokens
- **Network usage** drives token demand
- **Buyback mechanisms** from service revenue

---

## Service Pricing with CMX

### Payment Method Comparison

| Service Tier               | USD Price     | CMX Price        | Your Savings      |
| -------------------------- | ------------- | ---------------- | ----------------- |
| **Enterprise Self-Hosted** | $2,500/month  | 2,000 CMX/month  | **$500/month**    |
| **Managed (1 node)**       | $5,000/month  | 4,000 CMX/month  | **$1,000/month**  |
| **Managed (5 nodes)**      | $22,500/month | 15,000 CMX/month | **$6,300/month**  |
| **Managed (10 nodes)**     | $42,500/month | 26,000 CMX/month | **$11,900/month** |

_CMX service pricing is set by CapSign at a 20% discount to USD rates, providing immediate cost savings_

### Volume Discount Tiers

```
💰 CMX Token Volume Discounts

🥉 Base Tier (1-4 nodes)
├── USD discount: 0%
├── CMX discount: 20%
└── Effective savings: 20%

🥈 Silver Tier (5-9 nodes)
├── USD discount: 10%
├── CMX discount: 28% (20% + 10%)
└── Effective savings: 28%

🥇 Gold Tier (10-19 nodes)
├── USD discount: 15%
├── CMX discount: 32% (20% + 15%)
└── Effective savings: 32%

💎 Platinum Tier (20+ nodes)
├── USD discount: 25%
├── CMX discount: 40% (20% + 25%)
└── Effective savings: 40%
```

---

## Token Economics Model

### Supply & Distribution

```
🪙 CMX Token Supply: 1,000,000,000 CMX

📊 Distribution:
├── 25% - Community & Ecosystem (250M CMX)
├── 20% - Team & Advisors (200M CMX, 4-year vest)
├── 20% - Private Sale (200M CMX)
├── 15% - Public Sale (150M CMX)
├── 10% - Treasury (100M CMX)
├── 10% - Liquidity Rewards (100M CMX)
```

### Demand Drivers

1. **🏗️ Infrastructure Payments**

   - Monthly service fees in CMX
   - Growing enterprise adoption
   - Volume discount incentives

2. **⛽ Network Usage**

   - Gas fees for CMX Network transactions
   - DeFi applications on CMX Network
   - Capital markets activity

3. **🔒 Staking Rewards**

   - Protocol security staking
   - Validator node requirements
   - Governance participation

4. **💰 Value Accrual**
   - 50% of USD service revenue → CMX buybacks
   - Protocol fee sharing with stakers
   - Network growth value capture

---

## Service Pricing Mechanism

### CapSign-Controlled Pricing

**CapSign sets CMX service prices directly**, ensuring predictable enterprise billing:

1. **Fixed Service Rates**

   - CMX service prices set by CapSign treasury
   - Built-in 20% discount vs USD pricing
   - Stable, predictable monthly costs

2. **Enterprise Pricing Stability**

   - No market volatility impact on service costs
   - Annual rate locks available for enterprise contracts
   - Transparent pricing with advance notice of changes

---

## Token Acquisition Strategies

### For Enterprises

1. **📊 Treasury Purchase**

   - Direct purchase from CapSign treasury
   - OTC rates for large volumes
   - Structured payment plans available

2. **🔄 Dollar-Cost Averaging**

   - Monthly CMX purchases for service payments
   - Reduces volatility exposure
   - Automated buying programs

3. **💼 Service Credit Conversion**
   - Convert unused service credits to CMX
   - Lock in current pricing
   - Portfolio diversification

### For Developers

1. **🏊 DEX Trading**

   - Uniswap V3 (Ethereum & Optimism)
   - Balancer pools with stable liquidity
   - Best for smaller amounts

2. **⛏️ Network Participation**

   - Run CMX Network validators
   - Earn CMX through block rewards
   - Contribute to network security

3. **🎁 Ecosystem Rewards**
   - CapSign Protocol usage rewards
   - Community contribution bounties
   - Developer grant programs

---

## Value Proposition for Service Users

### Immediate Cost Savings

**Using Managed Service (2 nodes):**

- **USD cost**: $10,000/month
- **CMX cost**: 8,000 CMX/month 
- **Guaranteed savings**: $2,000/month (20% discount)

**Annual savings**: $24,000 per year with CMX payments

**Strategy**: Acquire CMX tokens through CapSign treasury or approved exchanges for consistent service cost savings

### Long-Term Value Proposition

1. **📈 Network Growth**: As CMX Network adoption grows, CMX value increases
2. **💰 Service Savings**: Immediate 20%+ cost reduction
3. **🎯 Ecosystem Alignment**: Support the technology you're building on
4. **🔮 Upside Potential**: Token appreciation as CapSign ecosystem expands

---

## Automated Payment Solutions

### Smart Contract Integration

```solidity
// Example: Automated CMX service payments
contract CapSignServicePayment {
    function payMonthlyService(uint256 nodes) external {
        uint256 cmxRequired = nodes * monthlyNodeCost;

        // Get current CMX/USD rate with 30-day average
        uint256 cmxPrice = priceOracle.getAveragePrice(30 days);

        // Apply volume discounts
        uint256 discount = getVolumeDiscount(nodes);
        uint256 finalCost = cmxRequired * (100 - discount) / 100;

        // Transfer CMX tokens
        cmxToken.transferFrom(msg.sender, treasury, finalCost);

        // Activate service
        serviceRegistry.activateService(msg.sender, nodes, 30 days);
    }
}
```

### Enterprise Treasury Management

- **🔄 Automated purchases** - DCA into CMX for service payments
- **📊 Treasury dashboard** - Real-time CMX balance and service costs
- **⚠️ Low balance alerts** - Automated notifications for token purchases
- **📈 Analytics** - Historical spending and savings tracking

---

## Getting Started with CMX Payments

### **Step 1: Acquire CMX Tokens**

1. **Small amounts**: [Uniswap](https://app.uniswap.org) or [Balancer](https://balancer.fi)
2. **Large amounts**: Contact [treasury@capsign.com](mailto:treasury@capsign.com)
3. **OTC desk**: [otc@capsign.com](mailto:otc@capsign.com) for $100K+ purchases

### **Step 2: Set Up Payment Method**

1. **Connect wallet** in CapSign dashboard
2. **Enable CMX payments** for your organization
3. **Set payment preferences** (auto-pay, price locks, etc.)

### **Step 3: Monitor & Optimize**

1. **Track savings** in billing dashboard
2. **Optimize timing** for token purchases
3. **Use volume discounts** as you scale

**Questions about CMX payments?** Contact [billing@capsign.com](mailto:billing@capsign.com)

---

_CMX token prices are illustrative. Actual savings depend on market conditions and volume tiers. Past performance does not guarantee future results._
