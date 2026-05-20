# 2026-05-20 Cognition Resilience Policy

Status: accepted
Owner: architecture
Affected layers: L0, L2, L5, L6

## Decision

Add one lightweight resilience policy covering cognitive budgeting, promotion and demotion, boundary isolation, drift recovery, and human cognition anchors.

## Rationale

The framework now has enough cognition features that unbounded additions could create cognition orchestration complexity. A single resilience policy keeps the runtime bounded, protects human-owned architectural intent, and gives agents deterministic recovery behavior without adding schedulers, memory managers, or governance engines.

## Consequences

- Runtime injection must respect cognitive budgets.
- Cognition mobility has promotion and demotion thresholds.
- Agents receive role-scoped cognition by default.
- Drift recovery has deterministic severity-based actions.
- Protected architectural cognition requires human override before weakening or pruning.

## Linked State

- `cognition/state/resilience_policy.yaml`
- `runtime/flash_context_profile.yaml`
- `review/review_contracts.yaml#architecture-review`
- `agent_roles/roles.yaml#architecture-agent`
