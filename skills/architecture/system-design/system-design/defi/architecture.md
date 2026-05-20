# System Design: DeFi Protocol (High Assurance)

## Context
You are a **Financial Systems Engineer**. Your priority is **Solvency, Security, and Atomic Composability**. You build on Sui because of its parallel execution, but you must architect around the "Shared Object" contention hotspots.

## Core Architecture: "The Liquidity Engine"

### 1. Order Book Integration (DeepBook V3)
*   **Pattern:** Do not build your own matching engine unless necessary. Compose with **DeepBook V3**.
*   **Architecture:**
    *   **Balance Manager:** Create a "Sub-Account" (Balance Manager) for the protocol/user to isolate trading funds from main wallet.
    *   **Routing:** Smart Order Router (off-chain SDK) calculates split across DeepBook pools and AMMs (Cetus, Aftermath).
    *   **Execution:** PTB (Programmable Transaction Block) executes `Swap -> FlashLoan -> Repay` atomically.

### 2. Oracle Redundancy (Price Feeds)
*   **Risk:** Stale prices lead to bad debt.
*   **Strategy:** **Medianizer Pattern** or **Pull-Based Oracles**.
    *   **Sources:** Integrate **Pyth** (Pull), **Switchboard**, and **Supra**.
    *   **Logic:** In your Move contract, require at least 2 sources to agree within 0.5% deviation.
    *   **Update:** Update price in the *same* PTB as the liquidation action to ensure freshness.

### 3. MEV Protection (Anti-Frontrunning)
*   **Problem:** Validators reordering transactions to extract value.
*   **Solutions on Sui:**
    *   **Fast Path (Mysticeti):** Low latency (390ms) reduces the window for MEV.
    *   **MEV-Aware RPCs:** Send transactions to private RPCs (like **Shio**) that promise transaction privacy until inclusion.
    *   **Slippage:** Enforce tight on-chain limits.

### 4. Contention Management
*   **Issue:** If 1000 users try to swap against the same Shared Object (Pool) continuously, they might serialize.
*   **Optimization:**
    *   **Sharded Counters:** If implementing a counter/limit, break it into N sub-objects.
    *   **Deferred Execution:** For heavy logic, split into "Request" (shared object interaction) and "Process" (crank turned by bot).

## Security Layers
*   **Emergency Brake:** A `PauseCap` held by a distinct multi-sig (or DAO) to freeze protocol interactions.
*   **Invariant Checks:** End of every transaction check: `assert!(reserves_before + deposit == reserves_after, ELeak)`.
*   **Reentrancy:** Move's resource model prevents most reentrancy, but "Hot Potato" pattern ensures flash loans are repaid.
