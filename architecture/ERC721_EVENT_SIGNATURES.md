# ERC-721 Event Signature Analysis

## The Problem

ERC-20, ERC-721, and ERC-7752 share event names but we discovered:

1. **Transfer**: 
   - ERC-20: `Transfer(address indexed from, address indexed to, uint256 value)` - NOT indexed
   - ERC-721: `Transfer(address indexed from, address indexed to, uint256 indexed tokenId)` - INDEXED
   - **Different signatures!** Can coexist but Solidity interface doesn't allow both

2. **Approval**:
   - ERC-20: `Approval(address indexed owner, address indexed spender, uint256 value)` - NOT indexed
   - ERC-721: `Approval(address indexed owner, address indexed approved, uint256 indexed tokenId)` - INDEXED
   - **Different signatures!** Can coexist but Solidity interface doesn't allow both

3. **ApprovalForAll**:
   - ERC-721: `ApprovalForAll(address indexed owner, address indexed operator, bool approved)`
   - ERC-7752: `ApprovalForAll(address indexed owner, address indexed operator, bool approved)`
   - **IDENTICAL! Can share!**

## Solution

Since ERC-20/ERC-7752 diamonds and ERC-721 diamonds will NEVER be the same diamond (function selector collision), we should:

1. **Keep events IN THE FACETS** (not in IToken interface)
2. **Use standard event names** for better ecosystem integration
3. **Share ApprovalForAll** (already in interface, identical signature)

This follows the standard exactly and integrates with existing tools (OpenSea, Etherscan, etc.)

## Implementation

- TokenERC20Facet: Declares `Transfer` and `Approval` events  
- TokenERC721Facet: Declares `Transfer` and `Approval` events (different signatures)
- TokenERC7752Facet: Uses `ApprovalForAll` from interface
- Token ERC721Facet: Uses `ApprovalForAll` from interface

This is the standard pattern and matches how OpenZeppelin and other implementations handle it.

