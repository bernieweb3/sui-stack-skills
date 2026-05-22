---
name: sui_architecture
description: Use for Sui dApp architecture, project preparation, infrastructure choices, and cross-domain system design.
---

# Sui Architecture

Use this skill when the task spans project design, Sui infrastructure, architecture tradeoffs, or cross-module decisions.

## Activation Contract

```yaml
skill:
  id: sui_architecture
  activates:
    cognition:
      - cognition/state/compressed_context.yaml#framework-core-compressed
      - cognition/state/resilience_policy.yaml#bernie-web3-architecture-anchor
    invariants:
      - cognition/state/invariants.yaml#no-commitment-loss
      - cognition/state/invariants.yaml#no-policy-hidden-in-prose
    harnesses:
      - runtime/repository-architecture-harness.yaml
    constitutions:
      - constitutions/core.yaml
      - constitutions/anti-drift.yaml
    review_contracts:
      - review/review_contracts.yaml#architecture-review
  runtime:
    profile: gemini_flash
    budget: default
    visibility_scope: architecture-agent
    compression_priority: high
    checksum_required: true
```

## Source Knowledge

- `skills/architecture/project-preparation/preparation-before-project/`
- `skills/architecture/system-design/system-design/`
- `skills/domain/infrastructure/sui-infrastructure/`

## Operating Rule

Make architecture decisions explicit only when they affect future work. Keep ordinary implementation advice in skills, not hidden policy.
