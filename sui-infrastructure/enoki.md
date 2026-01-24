# Skill: Enoki (Mysten Labs) Integration

## Context
You are a **Product Engineer** who wants "Web2-like onboarding" on Sui without building custom salt servers or proving backends. You use **Enoki**, the SaaS wrapper for zkLogin and Gas Station provided by Mysten Labs.

## Core Principles
1.  **Turnkey zkLogin:** Enoki handles the Salt Service and Proving Service for you.
2.  **Sponsored Transactions:** Enoki provides a simplified flow to sponsor gas for users, removing the "need SUI to start" friction.
3.  **API Key Security:** Enoki relies on a Public API key (safe for client) and Private Key (backend/admin).

## Technical Rules & Patterns

### 1. Enoki Flow (`@mysten/enoki`)
**Rule:** Initialize the `EnokiFlow` in your frontend.
```typescript
import { EnokiFlow } from '@mysten/enoki';

const enokiFlow = new EnokiFlow({
  apiKey: 'enoki_public_key_...',
});

// 1. Redirect to Google
enokiFlow.createZkLoginRequest({ provider: 'google' });

// 2. Handle Callback (on landing page)
await enokiFlow.handleAuthCallback();

// 3. Get Keypair
const keypair = await enokiFlow.getKeypair();
```

### 2. Sponsored Execution
**Rule:** Use Enoki's signer to execute transactions. It automatically appends the Gas Station object if configured.
```typescript
const tx = new Transaction();
tx.moveCall({ ... });

const res = await enokiFlow.executeTransactionBlock({
  transactionBlock: tx,
  network: 'mainnet',
});
```

### 3. Backend Integration (Optional)
**Context:** If you need to verify users on your backend.
**Rule:** Verify the JWT or the Enoki session token on your server using the Enoki Admin SDK/API to ensure the request truly comes from that user.

## Best Practices Checklist
- [ ] **Popups:** Enoki manages the OAuth popup window. Ensure your site is not blocking popups.
- [ ] **Session Persistence:** Enoki persists the ephemeral key in local storage. Handle "Logout" by calling `enokiFlow.logout()` to clear it.
- [ ] **Limits:** Be aware of Enoki's rate limits (free tier vs paid) for Proving and Gas sponsorship.
