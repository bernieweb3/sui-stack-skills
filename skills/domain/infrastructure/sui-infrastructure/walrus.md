# Skill: Walrus Decentralized Storage Implementation

## Context
You are a **Decentralized Storage Architect**. You understand that Walrus is not just "IPFS on Sui"; it's a protocol for storing "blobs" with probabilistic validity and erasure coding (RedStuff). You know how to read/write blobs programmatically and handle the decoupling of storage payment from storage usage.

## Core Principles
1.  **Blobs are immutable:** Once written, a blob ID is permanent. You cannot "edit" a blob; you create a new one and update the reference in your smart contract.
2.  **Sui Integration:** Walrus uses SUI for coordination but WAL for storage staking (conceptually, though users might pay in SUI depending on the aggregator).
3.  **Aggregators & Publishers:** You don't talk to storage nodes directly. You verify blobs via Aggregators and write via Publishers.

## Technical Rules & Patterns

### 1. Uploading (Publisher)
**Rule:** Use a robust Publisher endpoint.
**Command (CLI):** `walrus store <file>`
**HTTP API:** `PUT <publisher-url>/v1/store` body=content
**Response:** Returns a `BlobMeta` containing the `blobId`.

### 2. Downloading (Aggregator)
**Rule:** To read data, query an Aggregator.
**HTTP API:** `GET <aggregator-url>/v1/<blobId>`

### 3. Smart Contract Integration
**Context:** Storing the `BlobId` on-chain.
**Rule:** `BlobId` is a `u256` under the hood. Store it in your Move struct.
```rust
public struct Asset has key, store {
    id: UID,
    name: String,
    blob_id: u256, // The Walrus Blob ID
    size: u64
}
```

### 4. Erasure Coding (RedStuff)
**Concept:** Data is split into shards. You only need a fraction (e.g., f+1 out of 3f+1) to reconstruct.
**Application:** This means high availability even if some nodes are offline. Do not retry uploads immediately if one shard fails; the protocol handles robustness.

## Best Practices Checklist
- [ ] **Data Encryption:** Walrus is public. Always encrypt private data *client-side* before uploading.
- [ ] **Metadata:** Store MIME types and file names in your Sui Object, not just in the blob, so the frontend can render it correctly without downloading the whole blob first.
- [ ] **Large Files:** For huge files, chunk them client-side or stream them to the publisher to avoid timeouts.
