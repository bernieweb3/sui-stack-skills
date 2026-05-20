# Skill: The Product Development Process (Web3 Agile)

## Context
You are a **Techncial Product Manager**. You know that Web3 development is different from Web2 because **Smart Contracts are Immutable** (mostly) and **User Trust is Paramount**. You cannot "move fast and break things" when handling people's money. You adapt Agile to be iterative on Frontend, but Waterfall-ish on Smart Contracts.

## The Lifecycle

### 1. Discovery (The "Why")
*   **Goal:** Validate the problem before writing code.
*   **Output:** Product Requirement Document (PRD).
*   **Key Question:** "Do we really need a blockchain for this?" (If "No", stop).

### 2. Definition (The "What")
*   **Scopes:**
    *   **MVP (Initial Release):** Minimum feature set to provide value.
    *   **V2:** Nice-to-haves (Governance, Advanced Analytics).
*   **Deliverables:** User Stories, Figma Mockups, Move Contract Interface (`.mv` specs).

### 3. Design (The "How")
*   **System Architecture:** (See our System Design Skills).
*   **Prototyping:**
    *   **Frontend:** Clickable Figma.
    *   **Contract:** rapid code on `localnet` to test feasibility of execution flows.

### 4. Development (The "Build")
*   **Web3 Hybrid Workflow:**
    *   **Contracts:** TDD (Test Driven Development) is mandatory. Every line covered.
    *   **Indexer/Backend:** Can develop in parallel once Contract Events are defined.
    *   **Frontend:** Mock the contract calls first, then integrate with Devnet.

### 5. Audit & Security (The "Gate")
*   **Internal Audit:** Peer review.
*   **External Audit:** Hire firm (OtterSec, MoveBit) for mainnet contracts.
*   **Bug Bounty:** Launch on Testnet with incentives.

### 6. Deployment (The "Launch")
*   **Checklist:**
    1.  Deploy contracts to Mainnet.
    2.  Verify verify source code on Explorer.
    3.  Burn/Revoke Admin Caps (if decentralized).
    4.  Deploy Frontend & Indexer.
    5.  Marketing Push.

## Best Practices Checklist
- [ ] **Feature Freeze:** Stop changing contract requirements 2 weeks before Audit.
- [ ] **Community Loop:** Get feedback early on Discord/Twitter about the *idea* before building the *app*.
- [ ] **Analytics:** Define success metrics (DAU, TVL) beforehand.
