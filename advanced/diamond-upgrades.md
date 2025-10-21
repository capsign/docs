# Diamond Upgrades

Learn how to upgrade diamond contracts using the EIP-2535 Diamond Standard.

## Overview

The Diamond Pattern (EIP-2535) enables upgrading smart contracts by:

- Adding new facets (add functionality)
- Replacing facets (upgrade functionality)
- Removing facets (remove functionality)  
- Modifying function selectors

## Diamond Cut

Upgrades performed via `diamondCut` function. See [Protocol Documentation](/protocol/architecture.md) for detailed implementation.

## Safety

1. **Access Control** - Only authorized addresses can perform upgrades
2. **Testing** - Thoroughly test upgrades on testnet first
3. **Initialization** - Use initialization function for state migrations
4. **Immutable Storage** - Diamond storage layout must remain compatible

## Best Practices

- Test upgrades extensively on testnet
- Use multisig for upgrade authorization
- Document all changes
- Consider timelock for critical upgrades
- Have rollback plan

## Related Documentation

- [Protocol - Architecture](/protocol/architecture.md) - Diamond pattern details

---

**Next:** [Gas Optimization](/advanced/gas-optimization.md) →

