# Gas Optimization

Tips for optimizing gas costs when using CapSign.

## Account Abstraction Gas

### User Operation Gas Components

```
Total Gas = verificationGasLimit + callGasLimit + preVerificationGas
```

- **verificationGasLimit**: Gas for signature verification
- **callGasLimit**: Gas for actual transaction execution
- **preVerificationGas**: Fixed overhead for bundler

### Optimization Strategies

1. **Batch Operations**: Combine multiple operations into one UserOp
2. **Use Paymasters**: Let platform sponsor gas when possible
3. **Optimize Call Data**: Minimize data size
4. **Cache Off-Chain**: Store large data off-chain, only hashes on-chain

## Smart Contract Optimization

### Storage

- Use `uint256` instead of smaller types (unless packing)
- Pack struct variables efficiently
- Use mappings instead of arrays when possible
- Delete storage variables when no longer needed

### Functions

- Use `view`/`pure` when possible
- Avoid loops over unbounded arrays
- Use events instead of storing data on-chain
- Cache storage reads in memory

## Transaction Strategies

### Timing

- Send transactions during low-traffic periods
- Monitor gas prices and wait for dips
- Use gas price prediction APIs

### Batching

```typescript
// Instead of 3 separate transactions:
await send(tx1);
await send(tx2);
await send(tx3);

// Batch into one:
await sendUserOperation({
  calls: [tx1, tx2, tx3]
});
```

## Monitoring

Track your gas usage:

```typescript
const receipt = await waitForUserOperationReceipt({ hash });
const gasUsed = receipt.receipt.gasUsed;
const effectiveGasPrice = receipt.receipt.effectiveGasPrice;
const totalCost = gasUsed * effectiveGasPrice;

console.log(`Gas used: ${gasUsed}`);
console.log(`Total cost: ${formatEther(totalCost)} ETH`);
```

## Related Documentation

- [Protocol - Smart Accounts](/protocol/smart-accounts.md) - Account abstraction details

---

**Next:** [Troubleshooting](/advanced/troubleshooting.md) →

