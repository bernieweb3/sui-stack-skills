# Skill: Advanced Security Patterns (War Games)

## Context
You are a **Whitehat Hacker**. You look at code and see profit. Your job is to close the doors before the blackhats find them. You understand Move's type system is safe, but *logic* is where the bugs hide.

## Attack Vectors & Defenses

### 1. The Sandwich Attack (DeFi)
*   **Attack:** User submits a large swap. Attacker sees it in mempool (or anticipates it), buys before, sells after.
*   **Defense:**
    *   **Strict Slippage:** The contract must enforce a `min_out` argument.
    *   **Commit-Reveal:** For auctions, hide the bid value until the end.
    *   **Private RPCs:** Use solutions like **Shio** to route transactions privately to validators.

### 2. Oracle Manipulation
*   **Attack:** Attacker uses a Flash Loan to crash the price of SUI on a DEX, then liquidates a lending protocol that relies on that DEX's spot price.
*   **Defense:**
    *   **Time-Weighted Average Price (TWAP):** Never use Spot Price for collateral valuation.
    *   **Medianizer:** Compare prices from Pyth, Switchboard, and Supra. If one deviates > 1%, revert.

### 3. Flash Loan Reentrancy (Logic Level)
*   **Context:** Move has no dynamic dispatch reentrancy (like Solidity), but logical reentrancy exists.
*   **Attack:** User takes a Flash Loan, and inside the callback, interacts with the protocol's accounting in a way that assumes the loan is already paid.
*   **Defense:**
    *   **Reserves Check:** Always check `balance_before + loan_fee == balance_after` at the very end of the scope.
    *   **Hot Potato:** The `Receipt` struct must have no abilities. It forces consumption in the `repay` function.

## Safe Upgrade Patterns (Sui Specific)
*   **Risk:** You upgrade the package, but the new logic corrupts the old storage layout.
*   **Defense:**
    *   **Versioned Objects:** Add a `version: u64` field to your shared objects. Every upgrade increments the package version. Functions check `assert!(obj.version == PACKAGE_VERSION, EWrongVersion)`.
    *   **Append-Only:** Never remove fields from a Struct. Only add new dynamic fields or new structs.

## Best Practices Checklist
- [ ] **Fuzzing:** Use `move-prover` and rigorous property-based testing (fuzzing) to find edge cases.
- [ ] **Access Control revocation:** If an Admin Key is compromised, do you have a `EmergencyRotation` function that can be triggered by a DAO/Multisig?
- [ ] **Cap Isolation:** Don't put the `MintCap` and `UpgradeCap` in the same object. Separate concerns.
