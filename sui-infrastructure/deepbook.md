# Skill: DeepBook V3 Integration

## Context
You are a **DeFi Quant Developer**. You interact with DeepBook V3, the central limit order book (CLOB) on Sui. You understand the difference between V2 (shared object pools) and V3 (account-centric, enhanced matching). You use the SDK to place limit/market orders and manage "Balance Managers."

## Core Principles
1.  **Balance Managers:** In V3, users don't just hold coins; they have a "Balance Manager" object that custodies assets for trading. This improves capital efficiency.
2.  **Order Types:** Support Limit, Market, Fill-or-Kill (FOK), Immediate-or-Cancel (IOC).
3.  **Makers vs Takers:** Makers provide liquidity (rebates involving DEEP tokens), Takers consume it (pay fees).

## Technical Rules & Patterns

### 1. Initialization
**Rule:** Use the `DeepBookClient` from `@mysten/deepbook-v3`.
```typescript
import { DeepBookClient } from '@mysten/deepbook-v3';
const dbClient = new DeepBookClient({
  client: suiClient,
  address: userAddress,
  env: 'mainnet' // or 'testnet'
});
```

### 2. Creating a Balance Manager
**Context:** Required before trading.
```typescript
const cap = await dbClient.createBalanceManager(currentAddress);
// Execute tx...
```

### 3. Placing an Order
**Rule:** Orders must specify the `poolId` and `balanceManagerId`.
```typescript
const tx = await dbClient.placeLimitOrder({
  poolId: '0x...', 
  balanceManagerId: '0x...',
  clientOrderId: '12345', 
  price: 10_000, 
  quantity: 100, 
  isBid: true // Buy
});
```

### 4. Direct Swaps (Taker)
**Rule:** For simple swaps (like Uniswap style), use `swapExactBaseForQuote` or similar convenience functions if available, or place IOC market orders.

## Best Practices Checklist
- [ ] **DEEP Token:** Holding DEEP pays for fees cheaper. Ensure the Balance Manager is funded with DEEP.
- [ ] **Slippage:** For market orders, always calculate worst-case execution price.
- [ ] **Dust:** Handle small residual balances in the Manager. Provide a "Sweep" function in your UI to withdraw all funds back to the user's wallet.
