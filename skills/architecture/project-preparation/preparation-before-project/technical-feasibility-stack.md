# Skill: Technical Feasibility & Stack Selection

## Context
You are a **CTO**. You have a budget and a deadline. You need to pick the right tools. You don't pick a tech stack because it's "hype"; you pick it because it solves the problem with the least friction.

## Core Decisions

### 1. Build vs Buy (vs Fork)
*   **Buy (SaaS):** Use **Enoki** for Auth. Use **DeepBook** for liquidity. Use **Walrus** for storage.
    *   *Pros:* Fast/Cheap initially.
    *   *Cons:* Vendor lock-in.
*   **Build (Custom):** Write custom bespoke contracts.
    *   *Pros:* Full control. IP ownership.
    *   *Cons:* High risk (security), High cost (time).
*   **Fork:** Fork a verified repository (e.g., Uniswap v2 equivalent).
    *   *Pros:* Battle-tested logic.
    *   *Cons:* Might not fit unique needs.

### 2. Cost Estimations (Tokenomics of Dev)
*   **Gas Constraints:** Can we afford to store this data on-chain?
    *   *Rule:* If it doesn't need consensus, keep it off-chain (Walrus/IPFS/DB).
*   **Performance Budget:** Can the chain handle the throughput?
    *   *Sui:* Yes, generally. But consider RPC rate limits.

### 3. Repository Architecture
*   **Monorepo (`Turbo`, `Nx`):**
    *   `apps/web` (Frontend)
    *   `apps/indexer` (Backend)
    *   `packages/contracts` (Move)
    *   *Pros:* Unified versioning, shared types.
*   **Polyrepo:** Separate repos for Contract and Frontend.
    *   *Pros:* Clearer ownership if teams are totally separate.
    *   *Recommendation:* **Monorepo** is superior for Atomic deployments and Type consistency.

## The Default Protocol Stack (Sui)
*   **Smart Contracts:** Sui Move (2024).
*   **Frontend:** React + Vite + `@mysten/dapp-kit`.
*   **Backend:** Node.js (NestJS) or Rust (Axum).
*   **Indexer:** `sui-indexer-alt`.
*   **Storage:** PostgreSQL (Metadata), Walrus (Files).

## Best Practices Checklist
- [ ] **POC (Proof of Concept):** Before committing to a complex architecture, build a "Throwaway POC" in 2 days to test the riskiest assumption.
- [ ] **License Check:** Ensure libraries are compatible (MIT/Apache 2.0).
- [ ] **Team Skills:** Don't pick Rust for the backend if your team only knows Python, unless you have time to train.
