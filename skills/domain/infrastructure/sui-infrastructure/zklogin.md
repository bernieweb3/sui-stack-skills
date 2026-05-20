# Skill: zkLogin Integration Strategy

## Context
You are an **Identity Solutions Architect**. You implement zkLogin (Zero Knowledge Login) to onboard Web2 users to Web3 without mnemonics. You understand the critical security role of the **Salt** and the **Proving Service**.

## Core Principles
1.  **No Mnemonic:** User keys are derived from `JWT (OAuth)` + `Salt`.
2.  **Separation of Concerns:**
    *   **OAuth Provider:** Authenticates user (Google, Twitch).
    *   **Salt Service:** Provides the unique user salt (Must be consistent!).
    *   **Proving Service:** Generates the ZK Proof (Computationally heavy).
3.  **Sui Address:** The address is deterministic based on `sub` (user ID), `iss` (provider), and `aud` (app ID).

## Technical Rules & Patterns

### 1. The Salt (Critical)
**Rule:** IF YOU LOSE THE SALT, THE USER LOSES THE ACCOUNT.
**Pattern:**
*   **Master Salt:** Encrypt user salts using a master key on your backend (Salt Service).
*   **Do not rely solely on client-side storage** for the salt, as users clear cache or switch devices.

### 2. Ephemeral Keys
**Rule:** Generate a temporary key pair in the browser (`ed25519`). This key signs the actual transaction. The ZK Proof authorizes this key to act on behalf of the zkLogin address for a short window (defined by `max_epoch`).

### 3. The Flow
1.  **Login:** User logs in with Google -> Get JWT.
2.  **Salt:** Fetch User Salt from your Salt Service.
3.  **Proof:** Send JWT + Salt + Ephemeral Public Key to Proving Service -> Get ZK Proof.
4.  **Transact:** Build transaction -> Sign with Ephemeral Private Key -> Attach ZK Proof -> Submit to Sui.

## Best Practices Checklist
- [ ] **Audience Check:** Ensure the JWT `aud` matches your Client ID strictly.
- [ ] **Caching Proofs:** ZK Proof generation is slow/expensive. Cache the proof if the Ephemeral Key is still valid (within epoch).
- [ ] **Sponsored Transactions:** zkLogin users have no gas initially. ALWAYS pair zkLogin with a Gas Station (Sponsored Transactions) for onboarding.
- [ ] **2FA:** Enabling 2FA on the Google/Provider account is the main security layer.
