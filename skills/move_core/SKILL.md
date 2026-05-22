---
name: move_core
description: Use for Sui Move object modeling, package design, capability authority, shared-object decisions, and Move safety reviews.
---

# Move Core

Use this skill when the task touches Sui Move modules, object ownership, capabilities, shared objects, package upgrades, or Move tests.

## Activation Contract

```yaml
skill:
  id: move_core
  activates:
    cognition:
      - cognition/state/compressed_context.yaml#sui-transaction-compressed
      - cognition/state/architectural_commitments.yaml#sui-object-first-design
    invariants:
      - cognition/state/invariants.yaml#no-sui-account-model-drift
      - cognition/state/invariants.yaml#ptb-atomicity-visible
    harnesses:
      - harnesses/move.yaml
    constitutions:
      - constitutions/core.yaml
      - constitutions/sui-safety.yaml
    review_contracts:
      - review/review_contracts.yaml#move-safety-review
  runtime:
    profile: gemini_flash
    budget: gemini_flash
    visibility_scope: move-agent
    compression_priority: critical
    checksum_required: true
```

## Source Knowledge

- `skills/domain/sui/sui-stack-development/sui-move-smart-contract.md`
- `skills/domain/sui/sui-stack-development/sui-cli.md`

## Operating Rule

Start from Sui objects, capabilities, and PTB boundaries. Do not replace capability-first authority with raw sender checks unless a reviewed exception exists.
