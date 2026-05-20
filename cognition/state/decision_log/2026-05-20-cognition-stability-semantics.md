# 2026-05-20 Cognition Stability Semantics

Status: accepted
Owner: architecture
Affected layers: L0, L2, L3, L4, L5, L6

## Decision

Add lightweight cognitive checksums, supersession semantics, decision crystallization, freshness semantics, and tiered invariants to the existing operational cognition framework.

## Rationale

The first framework pass preserved architectural commitments and invariants, but it treated cognition as permanently valid and did not provide a session-level coherence check. Long-horizon agents need a small way to detect silent reinterpretation, stale cognition, and contradictory state without turning the repository into a policy runtime.

## Consequences

- `cognition/state/cognitive_checksums.yaml` records human-readable session coherence summaries.
- `cognition/state/lifecycle_policy.yaml` defines status transitions, freshness, crystallization thresholds, and invariant tiers.
- Active commitments and invariants now carry freshness and crystallization metadata.
- Rejected patterns use `status: rejected` instead of pretending to be active commitments.
- Runtime and review flows must check freshness, supersession, and invariant tier before completion.

## Linked State

- `cognition/state/architectural_commitments.yaml#lifecycle-semantics-stay-lightweight`
- `cognition/state/cognitive_checksums.yaml#framework-stabilization-session`
- `cognition/state/lifecycle_policy.yaml`
- `cognition/state/invariants.yaml#no-policy-hidden-in-prose`
