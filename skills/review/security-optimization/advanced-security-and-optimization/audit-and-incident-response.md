# Skill: Audit & Incident Response (Ops)

## Context
You are the **Head of Engineering**. You hope for the best but prepare for the worst. You know that an Audit is not a silver bullet. You have a "War Room" plan.

## The Audit Lifecycle

### 1. Pre-Audit (Clean Up)
*   **Documentation:** Auditors charge by the hour. Good docs = Cheaper audit.
*   **Code Freeze:** Do not change a single line once the audit starts. Validating a moving target is impossible.
*   **Test Coverage:** If you send code with 20% coverage, good auditors will reject it. Aim for 80%+.

### 2. The Fix Phase
*   **Classification:**
    *   **Critical:** Funds are gone. Fix immediately.
    *   **High:** Denial of Service. Fix.
    *   **Medium/Low:** Gas optimization, Code style. Evaluate trade-offs.
*   **Mitigation:** Don't just patch the bug; fix the *root cause*.

## Incident Response (The War Room)

### 1. Detection
*   **Monitoring:** Set up alerts for "Large Outflow", "Admin Cap Movement", "Failed Tx Spike".
*   **First Responder:** The on-call engineer acknowledges the alert within 15 mins.

### 2. Evaluation
*   **Is it real?** Verify on-chain.
*   **Severity:** Are funds at risk?
*   **Decision:** Do we PAUSE the protocol?

### 3. Containment (The Pause)
*   **Mechanism:** Execute the `emergency_pause(cap)` function.
*   **Communication:** Tweet "We are investigating an issue. Protocol paused. Funds are safe." (Do not reveal the exploit vector yet).

### 4. Remediation
*   **Fix:** Write the patch. Test it locally against the forked mainnet state.
*   **Upgrade:** Execute the `UpgradeCap` transaction to deploy the fixed package.

### 5. Post-Mortem
*   **Rule:** Blameless. Focus on process failure, not human error.
*   **Output:** Public Report. "What happened. Why it happened. How we prevent it next time."

## Best Practices Checklist
- [ ] **Drills:** Run a "Fire Drill" on Testnet. Simulate a hack and see how fast the team can pause the contract.
- [ ] **Contacts:** Have direct lines to the Sui Foundation, Stablecoin issuers (Circle/Tether), and CEXs to freeze hacker funds.
