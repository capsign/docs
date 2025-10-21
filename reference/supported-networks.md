# Supported Networks

Networks where CapSign protocol is deployed.

## Production Networks

### Base (L2)

- **Chain ID**: 8453
- **RPC**: https://mainnet.base.org
- **Explorer**: https://basescan.org
- **Native Token**: ETH
- **Status**: Primary production network

**Why Base?**
- Low transaction fees (~$0.01)
- Fast finality (~2 seconds)
- Ethereum security (Optimistic Rollup)
- Growing DeFi ecosystem

### Ethereum Mainnet

- **Chain ID**: 1
- **RPC**: https://eth.llamarpc.com
- **Explorer**: https://etherscan.io
- **Native Token**: ETH
- **Status**: Cross-chain operations only

## Testnets

### Base Sepolia

- **Chain ID**: 84532
- **RPC**: https://sepolia.base.org
- **Explorer**: https://sepolia.basescan.org
- **Faucet**: https://faucet.quicknode.com/base/sepolia
- **Status**: Primary testnet

### Ethereum Sepolia

- **Chain ID**: 11155111
- **RPC**: https://rpc.sepolia.org
- **Explorer**: https://sepolia.etherscan.io
- **Faucet**: https://sepoliafaucet.com
- **Status**: Cross-chain testing

## Network Configuration

### Wagmi Configuration

```typescript
import { base, baseSepolia } from "viem/chains";
import { http } from "viem";

export const config = createConfig({
  chains: [base, baseSepolia],
  transports: {
    [base.id]: http("https://mainnet.base.org"),
    [baseSepolia.id]: http("https://sepolia.base.org"),
  },
});
```

### Adding to MetaMask

**Base Mainnet**:
```
Network Name: Base
RPC URL: https://mainnet.base.org
Chain ID: 8453
Currency Symbol: ETH
Block Explorer: https://basescan.org
```

**Base Sepolia**:
```
Network Name: Base Sepolia
RPC URL: https://sepolia.base.org
Chain ID: 84532
Currency Symbol: ETH
Block Explorer: https://sepolia.basescan.org
```

## Future Networks

CapSign may expand to additional networks:

- Arbitrum
- Optimism
- Polygon
- Other EVM-compatible L2s

Check [Discord](https://discord.gg/gSmnZ9wmNv) for announcements.

## Network Status

Monitor network health:

- **Base**: [status.base.org](https://status.base.org)
- **Ethereum**: [ethstats.net](https://ethstats.net)

---

**Return to:** [Reference](/reference/) | [Documentation Home](/)

