# CMX Protocol Testing Guide

Comprehensive testing guide for developing and integrating with the CMX Protocol.

## Testing Overview

The CMX Protocol uses a multi-layered testing approach:

- **Unit Tests** - Individual contract function testing
- **Integration Tests** - Cross-contract interaction testing
- **End-to-End Tests** - Complete workflow testing
- **Security Tests** - Vulnerability and exploit testing
- **Gas Optimization Tests** - Performance and cost analysis

## Prerequisites

### Development Environment

```bash
# Install Foundry (required version 1.1.0+)
curl -L https://foundry.paradigm.xyz | bash
foundryup

# Install Node.js dependencies
npm install

# Install testing dependencies
npm install --save-dev @types/mocha @types/chai hardhat
```

### Test Configuration

```bash
# Copy test environment
cp .env.test.example .env.test

# Configure test settings
FORK_BLOCK_NUMBER=latest
FORK_URL=https://sepolia.base.org
TEST_MNEMONIC="test test test test test test test test test test test junk"
```

## Unit Testing

### Foundry Unit Tests

Located in `test/unit/`, these test individual contract functions in isolation.

#### Running Unit Tests

```bash
# Run all unit tests
forge test

# Run specific test file
forge test --match-path test/unit/AssetFactory.t.sol

# Run specific test function
forge test --match-test testCreateShareClass

# Run with verbosity
forge test -vvv

# Run with gas reporting
forge test --gas-report
```

#### Example Unit Test

```solidity
// test/unit/AssetFactory.t.sol
pragma solidity ^0.8.20;

import "forge-std/Test.sol";
import "../../src/Tokenization/factory/AssetFactory.sol";
import "../../src/Authorization/gam/GlobalAccessManager.sol";

contract AssetFactoryTest is Test {
    AssetFactory public factory;
    GlobalAccessManager public accessManager;

    address public admin = makeAddr("admin");
    address public user = makeAddr("user");

    function setUp() public {
        vm.startPrank(admin);

        accessManager = new GlobalAccessManager(admin);
        factory = new AssetFactory(address(accessManager));

        // Grant necessary roles
        accessManager.grantRole(ASSET_CREATOR_ROLE, user, 0);

        vm.stopPrank();
    }

    function testCreateShareClass() public {
        vm.startPrank(user);

        ShareClassConfig memory config = ShareClassConfig({
            votingRights: true,
            dividendRights: true,
            transferRestrictions: true,
            complianceRequired: true
        });

        address shareClass = factory.createShareClass(
            "Example Corp Class A",
            "EXMP-A",
            1000000,
            config
        );

        assertEq(factory.getAssetCount(), 1);
        assertTrue(factory.isValidAsset(shareClass));

        vm.stopPrank();
    }

    function testCreateShareClassUnauthorized() public {
        vm.startPrank(makeAddr("unauthorized"));

        ShareClassConfig memory config = ShareClassConfig({
            votingRights: true,
            dividendRights: true,
            transferRestrictions: true,
            complianceRequired: true
        });

        vm.expectRevert("AccessManager: account missing role");
        factory.createShareClass(
            "Example Corp Class A",
            "EXMP-A",
            1000000,
            config
        );

        vm.stopPrank();
    }

    function testFuzzCreateShareClassTotalShares(uint256 totalShares) public {
        vm.assume(totalShares > 0 && totalShares <= 1e18);
        vm.startPrank(user);

        ShareClassConfig memory config = ShareClassConfig({
            votingRights: true,
            dividendRights: true,
            transferRestrictions: false,
            complianceRequired: false
        });

        address shareClass = factory.createShareClass(
            "Test Corp",
            "TEST",
            totalShares,
            config
        );

        assertEq(IShareClass(shareClass).totalSupply(), totalShares);

        vm.stopPrank();
    }
}
```

### Contract Interaction Tests

#### Testing Diamond Upgrades

```solidity
// test/unit/DiamondUpgrade.t.sol
contract DiamondUpgradeTest is Test {
    Diamond public diamond;
    DiamondCutFacet public diamondCut;
    TestFacet public testFacet;

    function testAddFacet() public {
        // Deploy new facet
        testFacet = new TestFacet();

        // Prepare diamond cut
        FacetCut[] memory cuts = new FacetCut[](1);
        cuts[0] = FacetCut({
            facetAddress: address(testFacet),
            action: FacetCutAction.Add,
            functionSelectors: generateSelectors("TestFacet")
        });

        // Execute diamond cut
        diamondCut.diamondCut(cuts, address(0), "");

        // Verify facet was added
        address[] memory facets = DiamondLoupeFacet(address(diamond)).facets();
        assertTrue(contains(facets, address(testFacet)));
    }
}
```

## Integration Testing

### Cross-Contract Integration Tests

Located in `test/integration/`, these test complete workflows across multiple contracts.

#### Example Integration Test

```solidity
// test/integration/AssetCreationFlow.t.sol
contract AssetCreationFlowTest is Test {
    // All contracts needed for the flow
    AssetFactory public assetFactory;
    AttestationRegistry public attestationRegistry;
    DocumentRegistry public documentRegistry;
    GlobalAccessManager public accessManager;

    function testCompleteAssetCreationFlow() public {
        // 1. Set up KYC attestation
        bytes32 kycSchema = attestationRegistry.createSchema(
            "address verified, uint256 level, uint256 expiry",
            true
        );

        // 2. Create KYC attestation for user
        bytes32 attestationId = attestationRegistry.attest(
            kycSchema,
            user,
            abi.encode(user, 2, block.timestamp + 365 days)
        );

        // 3. Register asset documentation
        bytes32 documentHash = documentRegistry.registerDocument(
            keccak256("asset-prospectus"),
            "QmProspectusHash",
            DocumentType.PROSPECTUS
        );

        // 4. Create tokenized asset
        vm.startPrank(user);
        address asset = assetFactory.createShareClass(
            "Example Corp Class A",
            "EXMP-A",
            1000000,
            ShareClassConfig({
                votingRights: true,
                dividendRights: true,
                transferRestrictions: true,
                complianceRequired: true
            })
        );
        vm.stopPrank();

        // 5. Verify complete setup
        assertTrue(assetFactory.isValidAsset(asset));
        assertTrue(attestationRegistry.isValidAttestation(attestationId));
        assertTrue(documentRegistry.isValidDocument(documentHash));

        // 6. Test compliant transfer
        vm.prank(user);
        IShareClass(asset).transfer(anotherUser, 1000);

        assertEq(IShareClass(asset).balanceOf(anotherUser), 1000);
    }
}
```

### Fund Management Integration

```solidity
// test/integration/FundWorkflow.t.sol
contract FundWorkflowTest is Test {
    function testCompleteFundWorkflow() public {
        // 1. Create investment fund
        address fund = fundFactory.createUniversalFund(
            "Growth Equity Fund I",
            "GEF-I",
            FundConfig({
                managementFee: 200, // 2%
                performanceFee: 2000, // 20%
                lockupPeriod: 365 days,
                minimumInvestment: 1000000e6 // $1M USDC
            })
        );

        // 2. Investor deposits
        vm.startPrank(investor);
        usdc.approve(fund, 5000000e6); // $5M USDC
        uint256 shares = IUniversalFund(fund).deposit(5000000e6);
        vm.stopPrank();

        // 3. Fund makes investments
        vm.startPrank(fundManager);
        IUniversalFund(fund).invest(
            assetAddress,
            2000000e6 // $2M investment
        );
        vm.stopPrank();

        // 4. Time passes and performance occurs
        vm.warp(block.timestamp + 365 days);

        // Simulate 20% gain
        vm.mockCall(
            priceOracle,
            abi.encodeWithSelector(IPriceOracle.getPrice.selector, assetAddress),
            abi.encode(2400000e6) // $2.4M value
        );

        // 5. Calculate and distribute fees
        IUniversalFund(fund).calculateNAV();
        IUniversalFund(fund).processPerformanceFees();

        // 6. Investor redeems
        vm.startPrank(investor);
        uint256 redemptionAmount = IUniversalFund(fund).withdraw(shares);
        vm.stopPrank();

        // Verify net performance after fees
        assertGt(redemptionAmount, 5000000e6); // Positive return
        assertLt(redemptionAmount, 6000000e6); // Reasonable after fees
    }
}
```

## End-to-End Testing

### Full System Tests

#### Trading Workflow Test

```solidity
// test/e2e/TradingWorkflow.t.sol
contract TradingWorkflowTest is Test {
    function testCompleteSecuritiesTrading() public {
        // 1. Set up investors with KYC
        setupInvestorWithKYC(investor1);
        setupInvestorWithKYC(investor2);

        // 2. Company creates share class
        address shareClass = createCompanyShares();

        // 3. Company conducts securities offering
        address offering = createRule506bOffering(shareClass);

        // 4. Investors participate in offering
        participateInOffering(offering, investor1, 1000000e6);
        participateInOffering(offering, investor2, 500000e6);

        // 5. Offering closes and shares are distributed
        closeOffering(offering);

        // 6. Secondary trading begins
        address market = createSecondaryMarket(shareClass);

        // 7. Place and execute trades
        uint256 sellOrderId = placeSellOrder(market, investor1, 1000, 105e6);
        uint256 buyOrderId = placeBuyOrder(market, investor2, 500, 105e6);

        // 8. Verify trade execution
        assertEq(IShareClass(shareClass).balanceOf(investor1),
                 initialBalance - 500); // Sold 500 shares
        assertEq(IShareClass(shareClass).balanceOf(investor2),
                 initialBalance + 500); // Bought 500 shares

        // 9. Dividend distribution
        distributeDividend(shareClass, 10e6); // $10 per share

        // 10. Governance voting
        uint256 proposalId = createGovernanceProposal(shareClass);
        vote(proposalId, investor1, true);
        vote(proposalId, investor2, false);

        executeProposal(proposalId);
    }

    function setupInvestorWithKYC(address investor) internal {
        // Create KYC attestation
        vm.startPrank(kycProvider);
        bytes32 attestationId = attestationRegistry.attest(
            kycSchemaId,
            investor,
            abi.encode(investor, 2, block.timestamp + 365 days)
        );
        vm.stopPrank();

        // Verify attestation
        assertTrue(attestationRegistry.isValidAttestation(attestationId));
    }
}
```

## Security Testing

### Vulnerability Tests

#### Access Control Tests

```solidity
// test/security/AccessControl.t.sol
contract AccessControlTest is Test {
    function testUnauthorizedAccess() public {
        address attacker = makeAddr("attacker");

        // Try to create asset without permission
        vm.startPrank(attacker);
        vm.expectRevert("AccessManager: account missing role");
        assetFactory.createShareClass("Hack Corp", "HACK", 1000000, config);
        vm.stopPrank();

        // Try to upgrade diamond without permission
        vm.startPrank(attacker);
        vm.expectRevert("AccessManager: account missing role");
        diamondCut.diamondCut(maliciousCuts, address(0), "");
        vm.stopPrank();
    }

    function testPrivilegeEscalation() public {
        // User with limited role tries to grant themselves admin
        vm.startPrank(limitedUser);
        vm.expectRevert("AccessManager: account missing role");
        accessManager.grantRole(ADMIN_ROLE, limitedUser, 0);
        vm.stopPrank();
    }
}
```

#### Reentrancy Tests

```solidity
// test/security/Reentrancy.t.sol
contract ReentrancyTest is Test {
    function testReentrancyProtection() public {
        // Deploy malicious contract
        MaliciousReentrant attacker = new MaliciousReentrant(address(fund));

        // Attempt reentrancy attack
        vm.expectRevert("ReentrancyGuard: reentrant call");
        attacker.attack();
    }
}

contract MaliciousReentrant {
    IUniversalFund public target;

    constructor(address _target) {
        target = IUniversalFund(_target);
    }

    function attack() external {
        target.deposit(1000e6);
        target.withdraw(target.balanceOf(address(this)));
    }

    // Attempt to reenter during withdraw
    receive() external payable {
        if (address(target).balance > 0) {
            target.withdraw(target.balanceOf(address(this)));
        }
    }
}
```

### Fuzzing Tests

#### Property-Based Testing

```solidity
// test/security/Fuzzing.t.sol
contract FuzzingTest is Test {
    function testFuzzTransferInvariants(
        address from,
        address to,
        uint256 amount
    ) public {
        vm.assume(from != to);
        vm.assume(from != address(0) && to != address(0));
        vm.assume(amount > 0 && amount <= shareClass.balanceOf(from));

        uint256 fromBalanceBefore = shareClass.balanceOf(from);
        uint256 toBalanceBefore = shareClass.balanceOf(to);
        uint256 totalSupplyBefore = shareClass.totalSupply();

        vm.prank(from);
        shareClass.transfer(to, amount);

        // Invariants
        assertEq(shareClass.balanceOf(from), fromBalanceBefore - amount);
        assertEq(shareClass.balanceOf(to), toBalanceBefore + amount);
        assertEq(shareClass.totalSupply(), totalSupplyBefore);
    }

    function testFuzzOrderBookInvariants(
        uint256 price,
        uint256 amount
    ) public {
        vm.assume(price > 0 && price <= 1000e6); // Reasonable price range
        vm.assume(amount > 0 && amount <= 1000000); // Reasonable amount

        uint256 orderId = orderBook.placeBuyOrder(asset, amount, price);

        // Invariants
        assertTrue(orderBook.isValidOrder(orderId));
        assertEq(orderBook.getOrderPrice(orderId), price);
        assertEq(orderBook.getOrderAmount(orderId), amount);
    }
}
```

## Gas Optimization Testing

### Gas Usage Analysis

#### Gas Benchmarking

```solidity
// test/gas/GasBenchmark.t.sol
contract GasBenchmarkTest is Test {
    function testAssetCreationGas() public {
        uint256 gasStart = gasleft();

        address asset = assetFactory.createShareClass(
            "Gas Test Corp",
            "GAS",
            1000000,
            defaultConfig
        );

        uint256 gasUsed = gasStart - gasleft();

        // Assert reasonable gas usage
        assertLt(gasUsed, 500000); // Less than 500k gas

        emit log_named_uint("Asset creation gas", gasUsed);
    }

    function testBatchOperationsGas() public {
        // Single operations
        uint256 gasStart = gasleft();
        for (uint i = 0; i < 10; i++) {
            shareClass.transfer(makeAddr(string(abi.encode(i))), 100);
        }
        uint256 singleOpsGas = gasStart - gasleft();

        // Batch operation
        gasStart = gasleft();
        address[] memory recipients = new address[](10);
        uint256[] memory amounts = new uint256[](10);

        for (uint i = 0; i < 10; i++) {
            recipients[i] = makeAddr(string(abi.encode(i + 10)));
            amounts[i] = 100;
        }

        shareClass.batchTransfer(recipients, amounts);
        uint256 batchOpsGas = gasStart - gasleft();

        // Batch should be more efficient
        assertLt(batchOpsGas, singleOpsGas);

        emit log_named_uint("Single operations gas", singleOpsGas);
        emit log_named_uint("Batch operations gas", batchOpsGas);
        emit log_named_uint("Gas savings", singleOpsGas - batchOpsGas);
    }
}
```

## Hardhat Integration Tests

For JavaScript/TypeScript testing alongside Solidity tests.

### Setup

```typescript
// hardhat.config.ts
import { HardhatUserConfig } from "hardhat/config";
import "@nomicfoundation/hardhat-foundry";

const config: HardhatUserConfig = {
  solidity: "0.8.20",
  networks: {
    hardhat: {
      forking: {
        url: process.env.BASE_SEPOLIA_URL || "",
      },
    },
  },
};

export default config;
```

### JavaScript Integration Tests

```typescript
// test/integration/AssetManagement.test.ts
import { expect } from "chai";
import { ethers } from "hardhat";
import { CMXClient, Network } from "@capsign/sdk";

describe("Asset Management Integration", function () {
  let client: CMXClient;
  let deployer: any;
  let user1: any;
  let user2: any;

  beforeEach(async function () {
    [deployer, user1, user2] = await ethers.getSigners();

    client = new CMXClient({
      network: Network.HARDHAT,
      signer: deployer,
    });
  });

  it("Should create and transfer assets", async function () {
    // Create asset
    const asset = await client.assets.createShareClass({
      name: "Test Corp",
      symbol: "TEST",
      totalShares: 1000000,
      votingRights: true,
      dividendRights: true,
    });

    expect(asset.address).to.match(/^0x[a-fA-F0-9]{40}$/);

    // Transfer assets
    await client.assets.transfer({
      assetAddress: asset.address,
      to: user1.address,
      amount: ethers.utils.parseEther("1000"),
    });

    const balance = await client.assets.getBalance(
      asset.address,
      user1.address
    );

    expect(balance).to.equal(ethers.utils.parseEther("1000"));
  });

  it("Should handle compliance checks", async function () {
    // Create asset with compliance requirements
    const asset = await client.assets.createShareClass({
      name: "Compliance Corp",
      symbol: "COMP",
      totalShares: 1000000,
      complianceRequired: true,
    });

    // Attempt transfer without KYC (should fail)
    await expect(
      client.assets.transfer({
        assetAddress: asset.address,
        to: user1.address,
        amount: ethers.utils.parseEther("1000"),
      })
    ).to.be.revertedWith("Compliance check failed");

    // Add KYC attestation
    await client.compliance.createKYCAttestation({
      subject: user1.address,
      verificationLevel: "BASIC",
    });

    // Now transfer should succeed
    await client.assets.transfer({
      assetAddress: asset.address,
      to: user1.address,
      amount: ethers.utils.parseEther("1000"),
    });

    const balance = await client.assets.getBalance(
      asset.address,
      user1.address
    );

    expect(balance).to.equal(ethers.utils.parseEther("1000"));
  });
});
```

## Test Data Management

### Test Fixtures

```solidity
// test/utils/Fixtures.sol
library Fixtures {
    struct TestAsset {
        address asset;
        string name;
        string symbol;
        uint256 totalSupply;
    }

    function createBasicShareClass(
        AssetFactory factory,
        address creator
    ) internal returns (TestAsset memory) {
        vm.startPrank(creator);

        address asset = factory.createShareClass(
            "Test Corporation",
            "TEST",
            1000000,
            ShareClassConfig({
                votingRights: true,
                dividendRights: true,
                transferRestrictions: false,
                complianceRequired: false
            })
        );

        vm.stopPrank();

        return TestAsset({
            asset: asset,
            name: "Test Corporation",
            symbol: "TEST",
            totalSupply: 1000000
        });
    }

    function createKYCdInvestor(
        AttestationRegistry registry,
        address kycProvider,
        address investor
    ) internal returns (bytes32) {
        vm.startPrank(kycProvider);

        bytes32 attestationId = registry.attest(
            kycSchemaId,
            investor,
            abi.encode(investor, 2, block.timestamp + 365 days)
        );

        vm.stopPrank();

        return attestationId;
    }
}
```

## Continuous Integration

### GitHub Actions Workflow

```yaml
# .github/workflows/test.yml
name: Tests

on: [push, pull_request]

jobs:
  unit-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          submodules: recursive

      - name: Install Foundry
        uses: foundry-rs/foundry-toolchain@v1

      - name: Run unit tests
        run: forge test --match-path "test/unit/*"

      - name: Generate gas report
        run: forge test --gas-report

  integration-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          submodules: recursive

      - name: Install Foundry
        uses: foundry-rs/foundry-toolchain@v1

      - name: Run integration tests
        run: forge test --match-path "test/integration/*"

  security-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Install Foundry
        uses: foundry-rs/foundry-toolchain@v1

      - name: Run security tests
        run: forge test --match-path "test/security/*"

      - name: Run fuzzing tests
        run: forge test --match-path "test/security/*" --fuzz-runs 10000
```

## Best Practices

### Test Organization

- **Unit tests**: Focus on individual function behavior
- **Integration tests**: Test cross-contract interactions
- **End-to-end tests**: Test complete user workflows
- **Security tests**: Test for vulnerabilities and edge cases

### Test Data

- Use deterministic test data for consistent results
- Create reusable fixtures for common test scenarios
- Use fuzzing for edge case discovery
- Mock external dependencies for isolated testing

### Performance

- Optimize test execution time with parallel running
- Use caching for compiled contracts
- Minimize redundant setup in test suites
- Profile gas usage for optimization opportunities

This comprehensive testing guide ensures robust, secure, and efficient development of CMX Protocol integrations.
