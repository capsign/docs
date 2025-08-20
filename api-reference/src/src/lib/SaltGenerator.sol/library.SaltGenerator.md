# SaltGenerator
[Git Source](https://github.com/capsign/protocol/blob/dfa6820124c5610a6bfa06329447dbae7c24bc0a/src/lib/SaltGenerator.sol)

Unified salt generation for all CapSign contracts

*Provides standardized salt calculation methods for consistent address prediction*


## Functions
### generateWalletSalt

Generate salt for smart wallet deployment


```solidity
function generateWalletSalt(bytes[] memory owners, string memory walletType, uint256 nonce, address deployer)
    internal
    pure
    returns (bytes32 salt);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owners`|`bytes[]`|Array of owner public keys/addresses in bytes format|
|`walletType`|`string`|Type of wallet ("individual", "entity", "custodial", "multisig")|
|`nonce`|`uint256`|Deployment nonce for uniqueness|
|`deployer`|`address`|Address of the deployer (optional, use address(0) for deterministic)|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`salt`|`bytes32`|Deterministic salt for CREATE2|


### generateIAMSalt

Generate salt for IAM deployment


```solidity
function generateIAMSalt(address issuer, string memory issuerName, string memory templateName, uint256 nonce)
    internal
    pure
    returns (bytes32 salt);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`issuer`|`address`|Address of the issuer/entity|
|`issuerName`|`string`|Name of the issuer entity|
|`templateName`|`string`|Template name (optional)|
|`nonce`|`uint256`|Deployment nonce|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`salt`|`bytes32`|Deterministic salt for CREATE2|


### generateGovernanceSalt

Generate salt for governance deployment


```solidity
function generateGovernanceSalt(address creator, string memory governanceType, string memory name, uint256 nonce)
    internal
    pure
    returns (bytes32 salt);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`creator`|`address`|Address of the creator|
|`governanceType`|`string`|Type of governance|
|`name`|`string`|Governance name|
|`nonce`|`uint256`|Deployment nonce|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`salt`|`bytes32`|Deterministic salt for CREATE2|


### generateAssetSalt

Generate salt for asset/token deployment


```solidity
function generateAssetSalt(address issuer, string memory tokenSymbol, string memory tokenName, uint256 nonce)
    internal
    pure
    returns (bytes32 salt);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`issuer`|`address`|Address of the issuer|
|`tokenSymbol`|`string`|Token symbol|
|`tokenName`|`string`|Token name|
|`nonce`|`uint256`|Deployment nonce|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`salt`|`bytes32`|Deterministic salt for CREATE2|


### generateLedgerSalt

Generate salt for ledger deployment


```solidity
function generateLedgerSalt(address creator, string memory currencyCode, uint8 decimals, uint256 nonce)
    internal
    pure
    returns (bytes32 salt);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`creator`|`address`|Address of the creator|
|`currencyCode`|`string`|Currency code (e.g., "USD", "EUR")|
|`decimals`|`uint8`|Token decimals|
|`nonce`|`uint256`|Deployment nonce|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`salt`|`bytes32`|Deterministic salt for CREATE2|


### generateAASalt

Generate salt for Account Abstraction compatibility

*Used by toCapSignSmartAccount for interface integration*


```solidity
function generateAASalt(bytes[] memory ownersBytes, uint256 nonce) internal pure returns (bytes32 salt);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ownersBytes`|`bytes[]`|Packed owners bytes|
|`nonce`|`uint256`|Deployment nonce|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`salt`|`bytes32`|Deterministic salt for CREATE2|


### generateTestWalletSalt

Generate salt for test deployments with custom suffix

*Used in tests to ensure unique deployments*


```solidity
function generateTestWalletSalt(
    bytes[] memory owners,
    string memory walletType,
    uint256 nonce,
    address deployer,
    string memory customSuffix
) internal pure returns (bytes32 salt);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owners`|`bytes[]`|Array of owner public keys/addresses in bytes format|
|`walletType`|`string`|Type of wallet|
|`nonce`|`uint256`|Deployment nonce|
|`deployer`|`address`|Address of the deployer|
|`customSuffix`|`string`|Custom suffix for test uniqueness|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`salt`|`bytes32`|Deterministic salt for CREATE2|


