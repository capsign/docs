# FacetSelectors
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/lib/FacetSelectors.sol)

Library containing all facet selectors for deployment and testing

*This library provides a centralized location for all facet function selectors*


## Functions
### getWalletDeploymentSelectors

*Get WalletDeploymentFacet selectors*


```solidity
function getWalletDeploymentSelectors() internal pure returns (bytes4[] memory);
```

### getAssetDeploymentSelectors

*Get AssetDeploymentFacet selectors*


```solidity
function getAssetDeploymentSelectors() internal pure returns (bytes4[] memory);
```

### getLedgerDeploymentSelectors

*Get LedgerDeploymentFacet selectors*


```solidity
function getLedgerDeploymentSelectors() internal pure returns (bytes4[] memory);
```

### getOfferingDeploymentSelectors

*Get OfferingDeploymentFacet selectors*


```solidity
function getOfferingDeploymentSelectors() internal pure returns (bytes4[] memory);
```

### getFundDeploymentSelectors

*Get FundDeploymentFacet selectors*


```solidity
function getFundDeploymentSelectors() internal pure returns (bytes4[] memory);
```

### getGovernanceDeploymentSelectors

*Get GovernanceDeploymentFacet selectors*


```solidity
function getGovernanceDeploymentSelectors() internal pure returns (bytes4[] memory);
```

### getWalletConfigSelectors

*Get WalletConfigFacet selectors*


```solidity
function getWalletConfigSelectors() internal pure returns (bytes4[] memory);
```

### getAssetConfigSelectors

*Get AssetConfigFacet selectors*


```solidity
function getAssetConfigSelectors() internal pure returns (bytes4[] memory);
```

### getLedgerConfigSelectors

*Get LedgerConfigFacet selectors*


```solidity
function getLedgerConfigSelectors() internal pure returns (bytes4[] memory);
```

### getOfferingConfigSelectors

*Get OfferingConfigFacet selectors*


```solidity
function getOfferingConfigSelectors() internal pure returns (bytes4[] memory);
```

### getGovernanceConfigSelectors

*Get GovernanceConfigFacet selectors*


```solidity
function getGovernanceConfigSelectors() internal pure returns (bytes4[] memory);
```

### getGovernanceRegistrySelectors

*Get GovernanceRegistryFacet selectors*


```solidity
function getGovernanceRegistrySelectors() internal pure returns (bytes4[] memory);
```

### getGovernancePaymasterSelectors

*Get GovernancePaymasterFacet selectors*


```solidity
function getGovernancePaymasterSelectors() internal pure returns (bytes4[] memory);
```

### getAssetPaymasterSelectors

*Get AssetPaymasterFacet selectors*


```solidity
function getAssetPaymasterSelectors() internal pure returns (bytes4[] memory);
```

### getWalletPaymasterSelectors

*Get WalletPaymasterFacet selectors*


```solidity
function getWalletPaymasterSelectors() internal pure returns (bytes4[] memory);
```

### getPaymasterManagementSelectors

*Get PaymasterManagementFacet selectors*


```solidity
function getPaymasterManagementSelectors() internal pure returns (bytes4[] memory);
```

### getPaymasterValidationSelectors

*Get PaymasterValidationFacet selectors*


```solidity
function getPaymasterValidationSelectors() internal pure returns (bytes4[] memory);
```

### getLedgerPaymasterSelectors

*Get LedgerPaymasterFacet selectors*


```solidity
function getLedgerPaymasterSelectors() internal pure returns (bytes4[] memory);
```

### getOfferingPaymasterSelectors

*Get OfferingPaymasterFacet selectors*


```solidity
function getOfferingPaymasterSelectors() internal pure returns (bytes4[] memory);
```

### getDocumentCoreSelectors

*Get DocumentCoreFacet selectors*


```solidity
function getDocumentCoreSelectors() internal pure returns (bytes4[] memory);
```

### getDocumentSigningSelectors

*Get DocumentSigningFacet selectors*


```solidity
function getDocumentSigningSelectors() internal pure returns (bytes4[] memory);
```

### getDocumentTemplateSelectors

*Get DocumentTemplateFacet selectors*


```solidity
function getDocumentTemplateSelectors() internal pure returns (bytes4[] memory);
```

### getCoreWalletSelectors

*Get CoreWalletFacet selectors*


```solidity
function getCoreWalletSelectors() internal pure returns (bytes4[] memory);
```

### getWalletLedgerSelectors

*Get WalletLedgerFacet selectors*


```solidity
function getWalletLedgerSelectors() internal pure returns (bytes4[] memory);
```

### getDelegationSelectors

*Get DelegationFacet selectors*


```solidity
function getDelegationSelectors() internal pure returns (bytes4[] memory);
```

### getAssetCoreSelectors

*Get AssetCoreFacet selectors*


```solidity
function getAssetCoreSelectors() internal pure returns (bytes4[] memory);
```

### getTransferSelectors

*Get TransferFacet selectors*


```solidity
function getTransferSelectors() internal pure returns (bytes4[] memory);
```

### getLotManagementSelectors

*Get LotManagementFacet selectors*


```solidity
function getLotManagementSelectors() internal pure returns (bytes4[] memory);
```

### getComplianceSelectors

*Get ComplianceFacet selectors*


```solidity
function getComplianceSelectors() internal pure returns (bytes4[] memory);
```

### getAssetAdminSelectors

*Get AssetAdminFacet selectors*


```solidity
function getAssetAdminSelectors() internal pure returns (bytes4[] memory);
```

### getShareClassSelectors

*Get ShareClassFacet selectors*


```solidity
function getShareClassSelectors() internal pure returns (bytes4[] memory);
```

### getSafeSelectors

*Get SafeFacet selectors (Simple Agreement for Future Equity)*


```solidity
function getSafeSelectors() internal pure returns (bytes4[] memory);
```

### getSarSelectors

*Get SarFacet selectors (Stock Appreciation Rights)*


```solidity
function getSarSelectors() internal pure returns (bytes4[] memory);
```

### getWarrantSelectors

*Get WarrantFacet selectors*


```solidity
function getWarrantSelectors() internal pure returns (bytes4[] memory);
```

### getEsoSelectors

*Get EsoFacet selectors (Employee Stock Options)*


```solidity
function getEsoSelectors() internal pure returns (bytes4[] memory);
```

### getFundUnitSelectors

*Get FundUnitFacet selectors*


```solidity
function getFundUnitSelectors() internal pure returns (bytes4[] memory);
```

### getVestingSelectors

*Get VestingFacet selectors*


```solidity
function getVestingSelectors() internal pure returns (bytes4[] memory);
```

### getLockupSelectors

*Get LockupFacet selectors*


```solidity
function getLockupSelectors() internal pure returns (bytes4[] memory);
```

### getExerciseSelectors

*Get ExerciseFacet selectors*


```solidity
function getExerciseSelectors() internal pure returns (bytes4[] memory);
```

### getRule144Selectors

*Get Rule144Facet selectors*


```solidity
function getRule144Selectors() internal pure returns (bytes4[] memory);
```

### getAssetAttestationSelectors

*Get AttestationFacet selectors (Asset-specific attestations)*


```solidity
function getAssetAttestationSelectors() internal pure returns (bytes4[] memory);
```

### getSanctionsSelectors

*Get SanctionsFacet selectors*


```solidity
function getSanctionsSelectors() internal pure returns (bytes4[] memory);
```

### getOffChainAssetSelectors

*Get OffChainAssetFacet selectors*


```solidity
function getOffChainAssetSelectors() internal pure returns (bytes4[] memory);
```

### getAccountManagementSelectors

*Get AccountManagementFacet selectors*


```solidity
function getAccountManagementSelectors() internal pure returns (bytes4[] memory);
```

### getTransactionSelectors

*Get TransactionFacet selectors*


```solidity
function getTransactionSelectors() internal pure returns (bytes4[] memory);
```

### getQuerySelectors

*Get QueryFacet selectors*


```solidity
function getQuerySelectors() internal pure returns (bytes4[] memory);
```

### getLedgerAccessControlSelectors

*Get LedgerAccessControlFacet selectors*


```solidity
function getLedgerAccessControlSelectors() internal pure returns (bytes4[] memory);
```

### getUniversalFundCoreSelectors

*Get UniversalFundCoreFacet selectors*


```solidity
function getUniversalFundCoreSelectors() internal pure returns (bytes4[] memory);
```

### getCapitalAccountSelectors

*Get CapitalAccountFacet selectors*


```solidity
function getCapitalAccountSelectors() internal pure returns (bytes4[] memory);
```

### getCapitalCallSelectors

*Get CapitalCallFacet selectors*


```solidity
function getCapitalCallSelectors() internal pure returns (bytes4[] memory);
```

### getDistributionSelectors

*Get DistributionFacet selectors*


```solidity
function getDistributionSelectors() internal pure returns (bytes4[] memory);
```

### getAdvancedDistributionSelectors

*Get AdvancedDistributionFacet selectors*


```solidity
function getAdvancedDistributionSelectors() internal pure returns (bytes4[] memory);
```

### getInvestmentSelectors

*Get InvestmentFacet selectors*


```solidity
function getInvestmentSelectors() internal pure returns (bytes4[] memory);
```

### getDiamondCutSelectors

*Get DiamondCutFacet selectors*


```solidity
function getDiamondCutSelectors() internal pure returns (bytes4[] memory);
```

### getDiamondLoupeSelectors

*Get DiamondLoupeFacet selectors*


```solidity
function getDiamondLoupeSelectors() internal pure returns (bytes4[] memory);
```

### getOwnableSelectors

*Get OwnableFacet selectors*


```solidity
function getOwnableSelectors() internal pure returns (bytes4[] memory);
```

### getAccessControlSelectors

*Get AccessControlFacet selectors (core diamond access control)*


```solidity
function getAccessControlSelectors() internal pure returns (bytes4[] memory);
```

### getSchemaManagementSelectors

*Get SchemaManagementFacet selectors
Note: Excludes OpenZeppelin AccessManaged inherited functions (authority, setAuthority, isConsumingScheduledOp)
to avoid conflicts with AccessControlFacet. Only includes business logic functions.*


```solidity
function getSchemaManagementSelectors() internal pure returns (bytes4[] memory);
```

### getAttestationCoreSelectors

*Get AttestationCoreFacet selectors
Note: Excludes OpenZeppelin AccessManaged inherited functions (authority, setAuthority, isConsumingScheduledOp)
to avoid conflicts with AccessControlFacet. Only includes business logic functions.*


```solidity
function getAttestationCoreSelectors() internal pure returns (bytes4[] memory);
```

### getWebAuthnProxySelectors

*Get WebAuthnProxyFacet selectors
Note: Excludes OpenZeppelin AccessManaged inherited functions (authority, setAuthority, isConsumingScheduledOp)
to avoid conflicts with AccessControlFacet. Only includes business logic functions.*


```solidity
function getWebAuthnProxySelectors() internal pure returns (bytes4[] memory);
```

### getAttestationQuerySelectors

*Get AttestationQueryFacet selectors
Note: AttestationQueryFacet doesn't inherit from AccessManaged, so no conflicts*


```solidity
function getAttestationQuerySelectors() internal pure returns (bytes4[] memory);
```

### getAttestationAdminSelectors

*Get AttestationAdminFacet selectors
Note: Excludes OpenZeppelin AccessManaged inherited functions (authority, setAuthority, isConsumingScheduledOp, getAccessManager)
to avoid conflicts with AccessControlFacet. Only includes business logic functions.*


```solidity
function getAttestationAdminSelectors() internal pure returns (bytes4[] memory);
```

### getDocumentAdminSelectors

*Get DocumentAdminFacet selectors (same as AttestationAdminFacet)
Note: getAccessManager() removed to avoid conflict with AccessControlFacet*


```solidity
function getDocumentAdminSelectors() internal pure returns (bytes4[] memory);
```

### getFacetRegistryAdminSelectors

*Get FacetRegistry admin selectors (for permission configuration)*


```solidity
function getFacetRegistryAdminSelectors() internal pure returns (bytes4[] memory);
```

### getIdentityAccessManagerDeploymentSelectors

*Get IdentityAccessManagerDeploymentFacet selectors*


```solidity
function getIdentityAccessManagerDeploymentSelectors() internal pure returns (bytes4[] memory);
```

### getIdentityAccessManagerConfigSelectors

*Get IdentityAccessManagerConfigFacet selectors*


```solidity
function getIdentityAccessManagerConfigSelectors() internal pure returns (bytes4[] memory);
```

### getIdentityAccessManagerPaymasterSelectors

*Get IdentityAccessManagerPaymasterFacet selectors*


```solidity
function getIdentityAccessManagerPaymasterSelectors() internal pure returns (bytes4[] memory);
```

### getIdentityAccessManagerCoreSelectors

*Get IdentityAccessManagerCoreFacet selectors*


```solidity
function getIdentityAccessManagerCoreSelectors() internal pure returns (bytes4[] memory);
```

### getIdentityAccessManagerSubscriptionSelectors

*Get IdentityAccessManagerSubscriptionFacet selectors*


```solidity
function getIdentityAccessManagerSubscriptionSelectors() internal pure returns (bytes4[] memory);
```

### getIdentityAccessManagerWalletSelectors

*Get IdentityAccessManagerWalletFacet selectors*


```solidity
function getIdentityAccessManagerWalletSelectors() internal pure returns (bytes4[] memory);
```

### getIdentityAccessManagerGovernanceSelectors

*Get IdentityAccessManagerGovernanceFacet selectors*


```solidity
function getIdentityAccessManagerGovernanceSelectors() internal pure returns (bytes4[] memory);
```

### getFactoryBaseSelectors

*Get FactoryBaseFacet selectors*


```solidity
function getFactoryBaseSelectors() internal pure returns (bytes4[] memory);
```

### getBoardVotingSelectors

*Get BoardVotingFacet selectors*


```solidity
function getBoardVotingSelectors() internal pure returns (bytes4[] memory);
```

### getTokenholderVotingSelectors

*Get TokenholderVotingFacet selectors*


```solidity
function getTokenholderVotingSelectors() internal pure returns (bytes4[] memory);
```

### getGovernanceCoreSelectors

*Get GovernanceCoreFacet selectors*


```solidity
function getGovernanceCoreSelectors() internal pure returns (bytes4[] memory);
```

### getCompensationProposalSelectors

*Get CompensationProposalFacet selectors*


```solidity
function getCompensationProposalSelectors() internal pure returns (bytes4[] memory);
```

### getCommitteeManagementSelectors

*Get CommitteeManagementFacet selectors*


```solidity
function getCommitteeManagementSelectors() internal pure returns (bytes4[] memory);
```

### getGovernanceTimelockSelectors

*Get GovernanceTimelockFacet selectors*


```solidity
function getGovernanceTimelockSelectors() internal pure returns (bytes4[] memory);
```

