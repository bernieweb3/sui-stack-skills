# System Design: Consumer dApp (Invisible Web3)

## Context
You are a **Product Architect** building the next "Killer App" for millions of users. You know that popups, gas fees, and seed phrases are conversion killers. Your system hides the blockchain.

## Core Architecture: "The Invisible Wallet"

### 1. Identity & Auth (zkLogin)
*   **Pattern:** Use `zkLogin` to derive a non-custodial address from a Google/Apple JWT.
*   **Key Management:** Use **Enoki** or a custom **Salt Service** + **Ephemeral Keys**.
*   **Flow:**
    1.  User clicks "Login with Google".
    2.  App receives JWT.
    3.  App requests Salt (from Backend) + ZK Proof (from Proving Service).
    4.  App generates ephemeral session key pair in browser `sessionStorage`.
    5.  **User Effect:** Zero friction. No wallet extension required.

### 2. Gas Abstraction (Sponsored Tx)
*   **Pattern:** All user transactions are sent to a **Gas Station**.
*   **Component:** `Sui Gas Station` (API) holding a Gas Object.
*   **Flow:**
    1.  Browser constructs `TransactionBlock`.
    2.  Browser sends PTB to Gas Station API.
    3.  Gas Station signs as specific "Gas Owner" (Sponsor).
    4.  Browser signs with Ephemeral Key (Sender).
    5.  **Result:** User pays $0. App pays operational costs (opex).

### 3. Data Layer (Walrus)
*   **Problem:** Storing user avatars, post images, and videos on-chain is too expensive.
*   **Solution:** Use **Walrus** for decentralized blob storage.
*   **Pattern:**
    *   Upload media to Walrus Publisher -> Get `BlobId`.
    *   Store `BlobId` in the Sui Object (e.g., `Profile` object).
    *   Frontend queries Aggregator to render image: `https://aggregator.walrus-testnet.walrus.space/v1/<BlobId>`.

## Optimistic UI Updates
*   **Challenge:** Block time is ~400ms, but users expect 16ms response.
*   **Strategy:**
    1.  User clicks "Like".
    2.  UI immediately updates state (turns heart red).
    3.  Background: Sign & Submit transaction.
    4.  **Rollback:** If Tx fails (rare), revert UI and show toast "Action failed".

## Scalability Strategy
*   **Read Path:** Do NOT query the RPC for "All Posts". Use an **Indexer** (Sui Indexer Framework) to populate a specialized DB (Postgres/Redis) for feed generation.
*   **Write Path:** Direct RPC interaction for high throughput parallel writes (Sui's unique advantage).
