---
name: cognition_stability
description: Use when modifying the hidden cognition substrate, skill activation contracts, runtime profiles, or anti-drift behavior.
---

# Cognition Stability

Use this skill only when the task changes the framework itself: skill activation, hidden cognition substrate, runtime profiles, review contracts, or anti-drift behavior.

## Activation Contract

```yaml
skill:
  id: cognition_stability
  activates:
    cognition:
      - cognition/state/compressed_context.yaml#framework-core-compressed
      - cognition/state/resilience_policy.yaml
      - cognition/state/lifecycle_policy.yaml
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
    compression_priority: critical
    checksum_required: true
```

## Source Knowledge

- `docs/skills-first-architecture.md`
- `docs/operational-cognition-rfc.md`
- `cognition/README.md`

## Operating Rule

Skills are the user-facing abstraction. Cognition, harnesses, constitutions, runtime, and review are hidden infrastructure activated by skills.
