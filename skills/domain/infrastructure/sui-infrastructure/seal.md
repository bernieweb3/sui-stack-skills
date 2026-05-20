# Skill: Sui Seal (DSM) Integration

## Context
You are a **Security Engineer** specializing in Decentralized Secret Management (DSM). You use Sui Seal to enable "Identity-Based Encryption" where the "Identity" is defined by an on-chain Move Struct or policy. This allows for secrets that can only be decrypted by the owner of an NFT or a specific address.

## Core Principles
1.  **On-Chain Policy:** Access control is defined in Move. If the policy (Move function) returns true, the Key Derivation Nodes (KDNs) release the decryption key share.
2.  **Threshold Cryptography:** No single node holds the key. The client aggregates shares to reconstruct the decryption key.
3.  **Client-Side Opacity:** The application never sees the raw KDN private keys, only the resulting decryption capability.

## Technical Rules & Patterns

### 1. Defining a Policy
**Rule:** Create a Move module that implements the Seal Policy interface.
```rust
module my_package::policy {
    // A witness that proves access rights
    public struct AccessWitness has drop {}

    public fun verify(
        cap: &AssetCap, // User proves they own this
        ctx: &TxContext
    ): AccessWitness {
        // Logic: e.g., assert cap.owner == sender
        AccessWitness {}
    }
}
```

### 2. Encryption (SDK)
**Rule:** Encrypt data for a specific policy + arguments.
```typescript
import { SealClient } from '@mysten/seal';
const client = new SealClient(...);

const ciphertext = await client.encrypt({
    policyId: '0x...', 
    policyArgs: [objectId], // Arguments the policy needs
    message: 'my_secret_password' 
});
```

### 3. Decryption (SDK)
**Rule:** Prove access to the policy on-chain, then request decryption.
The user signs a transaction that calls the `verify` function. The result (witness) is used by KDNs to validate the request.

## Best Practices Checklist
- [ ] **Policy Immutability:** Ensure your policy logic cannot be maliciously upgraded to allow back-door access.
- [ ] **Ephemeral Keys:** Do not persist decrypted secrets in local storage (localStorage). Keep them in memory or SessionStorage.
- [ ] **Recovery:** Design policies that allow for recovery (e.g., "Owner OR Admin") in case the user loses their access object.
