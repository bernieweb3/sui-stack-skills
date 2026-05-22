---
name: ptb_runtime
description: Use for programmable transaction blocks, transaction sequencing, sponsorship, dry-runs, and atomic Sui flows.
---

# PTB Runtime

Use this skill when dependent Sui operations must be planned, signed, sponsored, dry-run, or executed atomically.

## Activation Contract

```yaml
skill:
  id: ptb_runtime
  activates:
    cognition:
      - cognition/state/compressed_context.yaml#sui-transaction-compressed
    invariants:
      - cognition/state/invariants.yaml#ptb-atomicity-visible
    harnesses:
      - harnesses/ptb.yaml
      - harnesses/frontend-transaction.yaml
    constitutions:
      - constitutions/core.yaml
      - constitutions/sui-safety.yaml
    review_contracts:
      - review/review_contracts.yaml#transaction-review
  runtime:
    profile: gemini_flash
    budget: gemini_flash
    visibility_scope: move-agent
    compression_priority: critical
    checksum_required: true
```

## Source Knowledge

- `skills/domain/sui/sui-stack-development/sui-cli.md`
- `skills/domain/infrastructure/sui-infrastructure/dapp-kit.md`

## Operating Rule

Document the PTB atomicity boundary before implementation. Do not split dependent value movement across transactions without decision rationale.
