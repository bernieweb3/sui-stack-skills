# System Design: GameFi & Metaverse

## Context
You are a **Game Engine Developer** bridging Unity/Unreal with Sui. You care about **Asset Ownership, Session Loops, and Latency**. You use Sui Kiosks for marketplaces and Dynamic Fields for growing items.

## Core Architecture: "The On-Chain Inventory"

### 1. Asset Management (Sui Kiosk)
*   **Pattern:** Use `Sui Kiosk` for all tradeable items (Characters, Weapons).
*   **Benefits:** Enforces **Royalties** at the protocol level.
*   **Structure:**
    *   `Hero` (Object) -> `Inventory` (Dynamic Bag) -> `Sword` (Object).
    *   When selling `Hero`, the `Sword` moves with it automatically (if wrapped correctly).

### 2. Dynamic Evolution (Dynamic Fields)
*   **Concept:** NFTs on Sui are not static JSON metadata; they are live objects.
*   **Pattern:**
    *   **Stats:** `strength`, `level` are struct fields.
    *   **Attachments:** Use `dynamic_object_field` to attach new capabilities (e.g., a "Gem" socketed into a "Sword").
    *   **Mutation:** Game server (or logic) sends transaction to mutate fields based on gameplay (XP gain).

### 3. Frictionless Play (Session Keys)
*   **Problem:** "Sign Transaction" popup for every sword swing is unplayable.
*   **Solution:** **Session Keys**.
    *   **Setup:** User signs *one* transaction: "Grant `GameServerPublicKey` permission to call `move_hero` function for 1 hour".
    *   **Loop:**
        1.  User presses 'W' (Move Forward).
        2.  Game Client signs payload with the ephemeral session key.
        3.  Game Client sends to RPC.
        4.  Move Contract verifies signature against the stored capability.

### 4. Hybrid Sync Strategy
*   **Authoritative Server:** For competitive logic (hit registration, physics), the Server is truth.
*   **On-Chain Sync:**
    *   **Frequency:** Sync "Important" state (Level Up, Loot Drop) to chain. Keep high-frequency state (position x,y,z) off-chain or in ephemeral streams/events.
    *   **Lag Compensation:** Optimistic client-side prediction, reconciled with on-chain state updates.

## Scalability
*   **Object Explosion:** Games create millions of objects.
*   **Cost Control:**
    *   Use `Burn` mechanics to recycle storage rebates.
    *   Combine assets (Crafting) to reduce object count.
