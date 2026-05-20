# Skill: Advanced Optimization Techniques (Squeezing Bits)

## Context
You are a **High-Frequency Trading (HFT) Engineer**. You care about gas to the micro-unit. You care about storage costs. You design for maximum parallelism.

## On-Chain Optimizations

### 1. Object Packing (Storage Rebates)
*   **Concept:** Sui charges for storage. It pays you back (rebate) when you delete objects.
*   **Strategy:**
    *   **Minimize Objects:** Instead of 1000 `Coin` objects, merge them into one `Coin`.
    *   **Field Packing:** Move aligns to bytes. `u8, u64` packs well.
    *   **Dynamic Fields vs Dynamic Object Fields:**
        *   `dynamic_field`: Stored inside the parent (cheaper access, harder to index).
        *   `dynamic_object_field`: Distinct child object (expensive access, easy to index).
        *   *Rule:* Use `dynamic_field` for small primitive data.

### 2. Parallel Execution Hacks
*   **Goal:** Avoid touching the same Shared Object (contention).
*   **Strategy:**
    *   **Sharding:** Liquidity Pool with 10 `Shards`. Users randomly pick a shard to deposit into.
    *   **Deferred Actions:** User writes to a "Request" object (Owned). A Keeper bot batches these requests and updates the Shared State in one go.

### 3. PTB (Programmable Transaction Block) Optimization
*   **Rule:** Do as much as possible in ONE transaction.
*   **Pattern:** `Swap A->B` + `Stake B` + `Transfer Receipt` = 1 Gas Fee, 1 Signature.
*   **Gas Budget:** Estimate accurately. Over-estimating locks up user SUI (though it is refunded, it affects User UX if they are low on funds).

## Off-Chain Optimizations

### 1. RPC Caching
*   **Problem:** `sui_getObject` is slow.
*   **Solution:**
    *   **Read-Through Cache:** Check Redis first. If miss, fetch RPC, write Redis (TTL 1 min).
    *   **Write-Through:** Indexer updates Redis immediately upon seeing the event.

### 2. Payload Size
*   **Rule:** Don't send massive Vectors as instruction arguments.
*   **Solution:** Upload data to blob storage (Walrus), put the Hash/ID in the transaction.

## Best Practices Checklist
- [ ] **Events Size:** Events cost gas. Don't emit the full state in the event. Emit the `ID` and `ChangeType`. Let the indexer look up the rest.
- [ ] **Loop Unrolling:** In Move, avoid massive loops. If you need to process 10,000 items, break it into 10 transactions of 1,000 items.
