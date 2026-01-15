# Platform Payments System

**Priority**: 🔥 HIGHEST  
**Status**: Ready to Implement  
**Estimated Time**: 1-2 days  
**Last Updated**: 2025-10-24

---

## Overview

Implement a revenue system for CapSign that charges fees for factory deployments (wallets, tokens, offerings, vehicles). This enables platform monetization and prepares for billing/invoicing.

---

## Components

### 1. Test USD Token (Base Sepolia)
Deploy a simple ERC20 token for testing payments on Base Sepolia.

### 2. Factory Payment Facets
Add payment functionality to revenue-generating factories:
- TokenFactory (charges for token creation)
- OfferingFactory (charges for offering creation)
- VehicleFactory (charges for fund/SPV creation, future)

**Note**: WalletFactory does NOT charge fees - wallets are free to create

### 3. Payment Configuration
Admin-managed fee configuration per factory.

### 4. UI Balance Display
Show payment token balance in the interface.

### 5. Future: Billing System
Track payments and generate invoices.

---

## Phase 1: Test USD Token Deployment

### Contract Specification

**File**: `protocol/src/test/MockUSD.sol`

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

/**
 * @title MockUSD
 * @notice Test USD token for Base Sepolia
 * 
 * ONLY FOR TESTING - DO NOT USE IN PRODUCTION
 */
contract MockUSD is ERC20, Ownable {
    uint8 private constant DECIMALS = 6; // Match USDC decimals
    
    constructor() ERC20("Mock USD", "mUSD") Ownable(msg.sender) {
        // Mint initial supply to deployer
        _mint(msg.sender, 1_000_000 * 10**DECIMALS); // 1M tokens
    }
    
    function decimals() public pure override returns (uint8) {
        return DECIMALS;
    }
    
    /**
     * @notice Mint tokens (only owner)
     */
    function mint(address to, uint256 amount) external onlyOwner {
        _mint(to, amount);
    }
    
    /**
     * @notice Faucet function for testing (anyone can claim)
     */
    function faucet() external {
        _mint(msg.sender, 10_000 * 10**DECIMALS); // 10k tokens per claim
    }
}
```

### Deployment Script

**File**: `protocol/script/deploy/DeployMockUSD.s.sol`

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

import "forge-std/Script.sol";
import "../../src/test/MockUSD.sol";

contract DeployMockUSD is Script {
    function run() external {
        uint256 deployerPrivateKey = vm.envUint("PRIVATE_KEY");
        address deployer = vm.addr(deployerPrivateKey);
        
        console.log("Deploying MockUSD to Base Sepolia");
        console.log("Deployer:", deployer);
        
        vm.startBroadcast(deployerPrivateKey);
        
        MockUSD token = new MockUSD();
        
        console.log("MockUSD deployed at:", address(token));
        console.log("Initial supply minted to:", deployer);
        console.log("Balance:", token.balanceOf(deployer));
        
        vm.stopBroadcast();
        
        // Save address to file
        string memory output = string.concat(
            '{\n',
            '  "mockUSD": "', vm.toString(address(token)), '",\n',
            '  "deployer": "', vm.toString(deployer), '",\n',
            '  "network": "base-sepolia"\n',
            '}'
        );
        
        vm.writeFile("./broadcast/DeployMockUSD.s.sol/base-sepolia/latest.json", output);
    }
}
```

### Deployment Commands

```bash
# Deploy to Base Sepolia
cd protocol
forge script script/deploy/DeployMockUSD.s.sol:DeployMockUSD \
  --rpc-url $BASE_SEPOLIA_RPC \
  --broadcast \
  --verify \
  -vvvv

# Mint additional tokens if needed
cast send <MOCK_USD_ADDRESS> \
  "mint(address,uint256)" \
  <YOUR_EOA> \
  10000000000 \
  --rpc-url $BASE_SEPOLIA_RPC \
  --private-key $PRIVATE_KEY
```

---

## Phase 2: Factory Payment Facets

### Architecture

Each factory will have an optional payment facet that:
1. Accepts payment tokens (USDC on mainnet, MockUSD on testnet)
2. Charges configurable fees for deployments
3. Tracks payments on-chain
4. Allows admin to withdraw collected fees
5. Allows admin to update fee configuration

### Storage Structure

**File**: `protocol/src/factories/facets/payment/FactoryPaymentStorage.sol`

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

library FactoryPaymentStorage {
    bytes32 internal constant STORAGE_SLOT = 
        keccak256("capsign.factory.payment.storage");
    
    struct Layout {
        // Payment configuration
        address paymentToken;        // USDC/MockUSD address
        uint256 deploymentFee;       // Fee per deployment (in token decimals)
        bool paymentsEnabled;        // Can disable temporarily
        
        // Fee recipient
        address feeRecipient;        // Where fees go (CapSign treasury)
        
        // Payment tracking
        mapping(address => uint256) totalPaidByUser;
        uint256 totalFeesCollected;
        uint256 totalDeployments;
        
        // Payment history (for invoicing)
        PaymentRecord[] payments;
        mapping(address => uint256[]) userPaymentIndices;
    }
    
    struct PaymentRecord {
        address payer;
        address deployment;      // Deployed contract address
        uint256 amount;
        uint256 timestamp;
        string deploymentType;   // "wallet", "token", "offering", "vehicle"
    }
    
    function layout() internal pure returns (Layout storage l) {
        bytes32 slot = STORAGE_SLOT;
        assembly {
            l.slot := slot
        }
    }
}
```

### Payment Facet Implementation

**File**: `protocol/src/factories/facets/payment/FactoryPaymentFacet.sol`

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
import "./FactoryPaymentStorage.sol";
import "../../../access/AccessManaged.sol";

/**
 * @title FactoryPaymentFacet
 * @notice Handles payment collection for factory deployments
 */
contract FactoryPaymentFacet is AccessManaged {
    using SafeERC20 for IERC20;
    
    // Events
    event PaymentConfigured(
        address indexed paymentToken,
        uint256 deploymentFee,
        address indexed feeRecipient
    );
    
    event PaymentReceived(
        address indexed payer,
        address indexed deployment,
        uint256 amount,
        string deploymentType,
        uint256 timestamp
    );
    
    event PaymentsToggled(bool enabled);
    
    event FeesWithdrawn(
        address indexed recipient,
        uint256 amount
    );
    
    // Errors
    error PaymentsDisabled();
    error InsufficientPayment();
    error PaymentFailed();
    error InvalidConfiguration();
    error NoFeesToWithdraw();
    
    /**
     * @notice Initialize payment configuration
     */
    function FactoryPayment_init(
        address paymentToken,
        uint256 deploymentFee,
        address feeRecipient
    ) external {
        FactoryPaymentStorage.Layout storage l = FactoryPaymentStorage.layout();
        
        if (paymentToken == address(0) || feeRecipient == address(0)) {
            revert InvalidConfiguration();
        }
        
        l.paymentToken = paymentToken;
        l.deploymentFee = deploymentFee;
        l.feeRecipient = feeRecipient;
        l.paymentsEnabled = true;
        
        emit PaymentConfigured(paymentToken, deploymentFee, feeRecipient);
    }
    
    /**
     * @notice Collect payment for deployment
     * @dev Called internally by factory during deployment
     */
    function _collectPayment(
        address payer,
        address deployment,
        string memory deploymentType
    ) internal {
        FactoryPaymentStorage.Layout storage l = FactoryPaymentStorage.layout();
        
        if (!l.paymentsEnabled) {
            revert PaymentsDisabled();
        }
        
        uint256 fee = l.deploymentFee;
        
        if (fee == 0) {
            return; // Free deployment
        }
        
        // Transfer payment from payer
        IERC20 token = IERC20(l.paymentToken);
        token.safeTransferFrom(payer, l.feeRecipient, fee);
        
        // Record payment
        l.totalPaidByUser[payer] += fee;
        l.totalFeesCollected += fee;
        l.totalDeployments++;
        
        // Store payment record
        uint256 index = l.payments.length;
        l.payments.push(FactoryPaymentStorage.PaymentRecord({
            payer: payer,
            deployment: deployment,
            amount: fee,
            timestamp: block.timestamp,
            deploymentType: deploymentType
        }));
        
        l.userPaymentIndices[payer].push(index);
        
        emit PaymentReceived(payer, deployment, fee, deploymentType, block.timestamp);
    }
    
    /**
     * @notice Update payment configuration (admin only)
     */
    function updatePaymentConfig(
        address paymentToken,
        uint256 deploymentFee,
        address feeRecipient
    ) external restricted {
        FactoryPaymentStorage.Layout storage l = FactoryPaymentStorage.layout();
        
        if (paymentToken == address(0) || feeRecipient == address(0)) {
            revert InvalidConfiguration();
        }
        
        l.paymentToken = paymentToken;
        l.deploymentFee = deploymentFee;
        l.feeRecipient = feeRecipient;
        
        emit PaymentConfigured(paymentToken, deploymentFee, feeRecipient);
    }
    
    /**
     * @notice Toggle payments on/off (admin only)
     */
    function togglePayments(bool enabled) external restricted {
        FactoryPaymentStorage.Layout storage l = FactoryPaymentStorage.layout();
        l.paymentsEnabled = enabled;
        
        emit PaymentsToggled(enabled);
    }
    
    /**
     * @notice Get current payment configuration
     */
    function getPaymentConfig() external view returns (
        address paymentToken,
        uint256 deploymentFee,
        address feeRecipient,
        bool paymentsEnabled
    ) {
        FactoryPaymentStorage.Layout storage l = FactoryPaymentStorage.layout();
        return (
            l.paymentToken,
            l.deploymentFee,
            l.feeRecipient,
            l.paymentsEnabled
        );
    }
    
    /**
     * @notice Get payment statistics
     */
    function getPaymentStats() external view returns (
        uint256 totalFeesCollected,
        uint256 totalDeployments
    ) {
        FactoryPaymentStorage.Layout storage l = FactoryPaymentStorage.layout();
        return (l.totalFeesCollected, l.totalDeployments);
    }
    
    /**
     * @notice Get user payment history
     */
    function getUserPaymentHistory(address user) 
        external 
        view 
        returns (FactoryPaymentStorage.PaymentRecord[] memory) 
    {
        FactoryPaymentStorage.Layout storage l = FactoryPaymentStorage.layout();
        uint256[] storage indices = l.userPaymentIndices[user];
        
        FactoryPaymentStorage.PaymentRecord[] memory records = 
            new FactoryPaymentStorage.PaymentRecord[](indices.length);
        
        for (uint256 i = 0; i < indices.length; i++) {
            records[i] = l.payments[indices[i]];
        }
        
        return records;
    }
    
    /**
     * @notice Get total amount paid by user
     */
    function getUserTotalPaid(address user) external view returns (uint256) {
        FactoryPaymentStorage.Layout storage l = FactoryPaymentStorage.layout();
        return l.totalPaidByUser[user];
    }
}
```

### Integration with Factories

Example integration with `TokenFactoryCoreFacet`:

```solidity
// In TokenFactoryCoreFacet.sol

import "../payment/FactoryPaymentFacet.sol";

function createToken(
    string memory name,
    string memory symbol,
    uint8 decimals,
    string memory baseURI
) external returns (address token) {
    // Collect payment BEFORE deployment
    FactoryPaymentFacet(address(this))._collectPayment(
        msg.sender,
        address(0), // Don't know address yet
        "token"
    );
    
    // ... existing deployment logic ...
    
    // Could update payment record with actual address if needed
}
```

**Note**: WalletFactory does NOT implement payment collection - wallets are free

---

## Phase 3: Update Contract Addresses

### Add Payment Token Addresses

**File**: `interface/src/constants/contracts.ts`

```typescript
const CONTRACT_ADDRESSES = {
  // ... existing addresses ...
  
  // Payment tokens
  PAYMENT_TOKEN: isBaseSepolia 
    ? "0x..." // MockUSD on Base Sepolia
    : "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913", // USDC on Base
  
  // Fee recipient (CapSign treasury)
  FEE_RECIPIENT: "0x...", // Your treasury address
};
```

---

## Phase 4: UI Balance Display

### Add Payment Balance Component

Create a new component to show payment token balance in the app header/navigation.

**File**: `interface/src/components/payments/PaymentTokenBalance.tsx`

```typescript
"use client";

import { useBalance, useChainId } from "wagmi";
import { baseSepolia, base } from "wagmi/chains";
import { useActiveWallet } from "@/contexts/ActiveWalletContext";
import CONTRACT_ADDRESSES from "@/constants/contracts";

export function PaymentTokenBalance() {
  const chainId = useChainId();
  const { activeWallet } = useActiveWallet();
  
  // Only show on Base Sepolia or Base
  if (chainId !== baseSepolia.id && chainId !== base.id) {
    return null;
  }
  
  const { data: balance, isLoading } = useBalance({
    address: activeWallet?.address as `0x${string}`,
    token: CONTRACT_ADDRESSES.PAYMENT_TOKEN as `0x${string}`,
    watch: true,
  });
  
  if (isLoading || !activeWallet) {
    return null;
  }
  
  const tokenSymbol = chainId === baseSepolia.id ? "mUSD" : "USDC";
  
  return (
    <div className="flex items-center gap-2 px-4 py-2 bg-gray-100 rounded-lg">
      <div className="text-sm text-gray-600">Platform Balance</div>
      <div className="font-semibold">
        {balance?.formatted || "0"} {tokenSymbol}
      </div>
      {chainId === baseSepolia.id && (
        <span className="text-xs text-gray-500">(Test)</span>
      )}
    </div>
  );
}
```

### Add to App Navigation

Add the balance display to your main navigation/header:

```typescript
// In your layout or header component
import { PaymentTokenBalance } from "@/components/payments/PaymentTokenBalance";

export function AppHeader() {
  return (
    <header>
      {/* Existing header content */}
      
      {/* Add payment balance */}
      <PaymentTokenBalance />
      
      {/* Rest of header */}
    </header>
  );
}
```

---

## Phase 5: Factory Fee Management UI

### Admin Payment Configuration Page

**File**: `interface/src/app/admin/payments/page.tsx`

```typescript
"use client";

import { useState } from "react";
import { useReadContract, useWriteContract } from "wagmi";
import CONTRACT_ADDRESSES from "@/constants/contracts";
import { parseUnits } from "viem";

export default function PaymentsAdminPage() {
  const [newFee, setNewFee] = useState("");
  
  // Read current config
  const { data: config } = useReadContract({
    address: CONTRACT_ADDRESSES.WALLET_FACTORY,
    abi: [/* FactoryPaymentFacet ABI */],
    functionName: "getPaymentConfig",
  });
  
  const { writeContract } = useWriteContract();
  
  const updateFee = () => {
    writeContract({
      address: CONTRACT_ADDRESSES.WALLET_FACTORY,
      abi: [/* FactoryPaymentFacet ABI */],
      functionName: "updatePaymentConfig",
      args: [
        config?.paymentToken,
        parseUnits(newFee, 6), // USDC has 6 decimals
        config?.feeRecipient,
      ],
    });
  };
  
  return (
    <div>
      <h1>Payment Configuration</h1>
      
      <div>
        <h2>Current Settings</h2>
        <p>Payment Token: {config?.paymentToken}</p>
        <p>Deployment Fee: {config?.deploymentFee?.toString()} (raw)</p>
        <p>Fee Recipient: {config?.feeRecipient}</p>
        <p>Payments Enabled: {config?.paymentsEnabled ? "Yes" : "No"}</p>
      </div>
      
      <div>
        <h2>Update Fee</h2>
        <input 
          type="number" 
          value={newFee}
          onChange={(e) => setNewFee(e.target.value)}
          placeholder="Enter fee (e.g. 1000)"
        />
        <button onClick={updateFee}>Update Fee</button>
      </div>
    </div>
  );
}
```

---

## Phase 6: Frontend Payment Flow

### Update Factory Hooks

**Example**: Update `useWalletFactory` to handle payments

```typescript
export function useCreateWallet() {
  const { writeContract } = useWriteContract();
  
  // Check if user has approved payment token
  const { data: allowance } = useReadContract({
    address: CONTRACT_ADDRESSES.PAYMENT_TOKEN,
    abi: erc20Abi,
    functionName: "allowance",
    args: [userAddress, CONTRACT_ADDRESSES.WALLET_FACTORY],
  });
  
  // Approve payment token if needed
  const approvePayment = async (amount: bigint) => {
    await writeContract({
      address: CONTRACT_ADDRESSES.PAYMENT_TOKEN,
      abi: erc20Abi,
      functionName: "approve",
      args: [CONTRACT_ADDRESSES.WALLET_FACTORY, amount],
    });
  };
  
  const createWallet = async (config: WalletConfig) => {
    // Step 1: Get current fee
    const fee = await readContract({
      address: CONTRACT_ADDRESSES.WALLET_FACTORY,
      abi: factoryPaymentAbi,
      functionName: "getPaymentConfig",
    });
    
    // Step 2: Approve if needed
    if (allowance < fee.deploymentFee) {
      await approvePayment(fee.deploymentFee);
    }
    
    // Step 3: Create wallet (payment will be collected automatically)
    await writeContract({
      address: CONTRACT_ADDRESSES.WALLET_FACTORY,
      abi: walletFactoryAbi,
      functionName: "createWallet",
      args: [config],
    });
  };
  
  return { createWallet };
}
```

---

## Phase 7: Payment Event Tracking (Future: Billing System)

### Subgraph Schema Updates

**File**: `subgraph/schema.graphql`

```graphql
# Payment tracking
type FactoryPayment @entity {
  id: ID!                              # transaction hash + log index
  payer: Wallet!
  deployment: Bytes!                   # Deployed contract address
  amount: BigInt!
  timestamp: BigInt!
  deploymentType: String!              # "wallet", "token", "offering", "vehicle"
  transactionHash: Bytes!
  
  # Invoice generation
  invoiced: Boolean!
  invoice: Invoice
}

type Invoice @entity {
  id: ID!                              # Generated invoice ID
  user: Wallet!
  payments: [FactoryPayment!]!
  totalAmount: BigInt!
  currency: String!                    # "USDC" or "mUSD"
  period: String!                      # "2024-10" 
  generatedAt: BigInt!
  pdfUrl: String                       # IPFS link to PDF invoice
}
```

### Invoice Generation (Future)

```typescript
// Generate monthly invoices
async function generateMonthlyInvoices() {
  // Query payments from subgraph
  const payments = await fetchPaymentsForMonth();
  
  // Group by user
  const paymentsByUser = groupBy(payments, 'payer');
  
  // Generate invoice for each user
  for (const [user, userPayments] of paymentsByUser) {
    const invoice = {
      user,
      payments: userPayments,
      totalAmount: sum(userPayments.map(p => p.amount)),
      period: getCurrentMonth(),
      generatedAt: Date.now(),
    };
    
    // Generate PDF
    const pdf = await generateInvoicePDF(invoice);
    
    // Upload to IPFS
    const ipfsHash = await uploadToIPFS(pdf);
    
    // Save to database
    await saveInvoice({ ...invoice, pdfUrl: ipfsHash });
    
    // Send email notification
    await sendInvoiceEmail(user, ipfsHash);
  }
}
```

---

## Implementation Checklist

### ✅ Phase 1: Test Token (2 hours)
- [ ] Create `MockUSD.sol` contract
- [ ] Create deployment script
- [ ] Deploy to Base Sepolia
- [ ] Verify on Basescan
- [ ] Mint tokens to your EOA
- [ ] Test faucet function
- [ ] Update contract addresses file

### ✅ Phase 2: Payment Facets (4 hours)
- [ ] Create `FactoryPaymentStorage.sol`
- [ ] Create `FactoryPaymentFacet.sol`
- [ ] Write tests for payment facet
- [ ] Deploy facet to Base Sepolia
- [ ] Integrate with `TokenFactoryCoreFacet`
- [ ] Integrate with `OfferingFactoryCoreFacet`
- [ ] Deploy updated factories (Token & Offering only)

### ✅ Phase 3: Factory Upgrades (2 hours)
- [ ] Add payment facet to TokenFactory diamond
- [ ] Add payment facet to OfferingFactory diamond
- [ ] Initialize with payment config (1000 mUSD on testnet, 1000 USDC on mainnet)
- [ ] ~~Add to WalletFactory~~ - **Wallets are FREE** ✅
- [ ] Test payment collection

### ✅ Phase 4: UI Updates (3 hours)
- [ ] Create `PaymentTokenBalance` component
- [ ] Add to app header/navigation
- [ ] Update factory hooks to handle approvals
- [ ] Update factory hooks to show fee requirements
- [ ] Add payment confirmation modal
- [ ] Test full payment flow

### ✅ Phase 5: Admin UI (2 hours)
- [ ] Create admin payments page
- [ ] Add fee configuration form
- [ ] Add payment statistics dashboard
- [ ] Add ability to toggle payments
- [ ] Test admin functions

### ✅ Phase 6: Testing (2 hours)
- [ ] Test MockUSD faucet
- [ ] Test token approval
- [ ] Test token creation with payment
- [ ] Test offering creation with payment
- [ ] ~~Test wallet creation with payment~~ - **Wallets are FREE** ✅
- [ ] Test admin fee updates
- [ ] Test payment history queries

### 🔮 Future: Billing System
- [ ] Update subgraph schema
- [ ] Index payment events
- [ ] Build invoice generation service
- [ ] Create invoice PDF templates
- [ ] Set up automated monthly invoicing
- [ ] Add invoice viewing in UI

---

## Configuration Reference

### Base Sepolia (Testing)
- **Payment Token**: MockUSD (to be deployed)
- **Deployment Fee**: 1000 mUSD (1000000000 in raw units, 6 decimals)
- **Fee Recipient**: Your treasury wallet

### Base Mainnet (Production)
- **Payment Token**: USDC `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913`
- **Deployment Fee**: 1000 USDC (1000000000 in raw units, 6 decimals)
- **Fee Recipient**: CapSign treasury wallet

---

## Revenue Projections

### Example Pricing
- Wallet Creation: **FREE** ✅
- Token Creation: $1000
- Offering Creation: $500
- Vehicle Creation: $1000 (future)

### Monthly Revenue Estimate
If you have:
- 10 companies creating tokens = $10,000
- 20 offerings created = $10,000
- ~~100 wallets created~~ = FREE

**Total**: ~$20,000/month

---

## Security Considerations

1. **Payment Token Validation**: Always validate payment token address
2. **Fee Updates**: Only admin can update fees
3. **Reentrancy**: Use SafeERC20 for all token transfers
4. **Emergency Stop**: Admin can disable payments if needed
5. **Treasury Security**: Use multi-sig for fee recipient address

---

## Testing Strategy

1. **Unit Tests**: Test payment facet functions in isolation
2. **Integration Tests**: Test full deployment flow with payments
3. **E2E Tests**: Test UI → approval → payment → deployment flow
4. **Manual Testing**: Test on Base Sepolia before mainnet

---

## Related Documents

- [Interface Refactoring Plan](./INTERFACE_REFACTORING_PLAN.md)
- [Token Diamond Architecture](./TOKEN_DIAMOND_ARCHITECTURE.md)
- [Factory Architecture](../protocol/README.md)

---

## Notes

- Start with Base Sepolia testing only
- Deploy MockUSD first, test thoroughly
- Add payment facets to one factory at a time
- Test each integration before moving to next
- Base mainnet deployment comes after thorough testing
- Billing system is Phase 2 (after payment collection works)

