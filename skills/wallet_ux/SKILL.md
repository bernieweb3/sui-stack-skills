---
name: wallet_ux
description: Use for wallet connection, dApp Kit usage, transaction UX, signing states, and frontend finality handling.
---

# Wallet UX

Use this skill when the task touches wallet connection, transaction prompts, user approval, network handling, transaction state, or frontend Sui interactions.

## Activation Contract

```yaml
skill:
  id: wallet_ux
  activates:
    cognition:
      - cognition/state/compressed_context.yaml#sui-transaction-compressed
    invariants:
      - cognition/state/invariants.yaml#ptb-atomicity-visible
    harnesses:
      - harnesses/frontend-transaction.yaml
    constitutions:
      - constitutions/core.yaml
    review_contracts:
      - review/review_contracts.yaml#transaction-review
  runtime:
    profile: gemini_flash
    budget: gemini_flash
    visibility_scope: frontend-agent
    compression_priority: high
    checksum_required: true
```

## Source Knowledge

- `skills/domain/infrastructure/sui-infrastructure/dapp-kit.md`
- `skills/implementation/frontend/frontend-development/reactjs.md`
- `skills/implementation/frontend/frontend-development/nextjs.md`

## Operating Rule

Never fake finality. Preserve explicit wallet rejection, pending, failure, success, digest, and network state.
