---
name: security_audit
description: Use for financial safety, authorization review, audit readiness, incident response, and invariant preservation.
---

# Security Audit

Use this skill when the task touches funds, admin authority, protocol invariants, pause/upgrade paths, audits, or incident response.

## Activation Contract

```yaml
skill:
  id: security_audit
  activates:
    cognition:
      - cognition/state/compressed_context.yaml#sui-transaction-compressed
      - cognition/state/resilience_policy.yaml#bernie-web3-architecture-anchor
    invariants:
      - cognition/state/invariants.yaml#financial-safety-before-optimization
      - cognition/state/invariants.yaml#no-sui-account-model-drift
    harnesses:
      - harnesses/financial-safety.yaml
    constitutions:
      - constitutions/core.yaml
      - constitutions/sui-safety.yaml
    review_contracts:
      - review/review_contracts.yaml#financial-review
  runtime:
    profile: gemini_flash
    budget: default
    visibility_scope: audit-agent
    compression_priority: critical
    checksum_required: true
```

## Source Knowledge

- `skills/review/security-optimization/advanced-security-and-optimization/advanced-security-patterns.md`
- `skills/review/security-optimization/advanced-security-and-optimization/audit-and-incident-response.md`
- `skills/architecture/system-design/system-design/defi/architecture.md`

## Operating Rule

Safety beats optimization. Do not weaken solvency, authority, oracle, pause, or PTB guarantees for gas, speed, or cleaner abstractions.
