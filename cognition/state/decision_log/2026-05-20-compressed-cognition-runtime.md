# 2026-05-20 Compressed Cognition Runtime

Status: accepted
Owner: architecture
Affected layers: L0, L2, L4, L5

## Decision

Add lightweight context compression, cognition conflict detection, reasoning snapshots, cognition weighting, and drift metrics to the operational cognition framework.

## Rationale

The framework now preserves cognition, but long sessions can still accumulate context overhead and cognition entropy. Agents need compressed working sets, explicit conflict records, concise reasoning topology, weight-aware injection, and measurable drift signals without introducing a policy engine or graph runtime.

## Consequences

- `compressed_context.yaml` provides route-sized cognition packets.
- `conflict_registry.yaml` records recurring or unresolved cognitive tensions.
- `reasoning_snapshots.yaml` preserves reasoning shape without storing reasoning transcripts.
- `drift_metrics.yaml` defines lightweight operational stability metrics.
- Cognition weighting lives in `lifecycle_policy.yaml` to avoid another registry.

## Linked State

- `cognition/state/compressed_context.yaml#framework-core-compressed`
- `cognition/state/conflict_registry.yaml#lightweight-runtime-vs-stability-metadata`
- `cognition/state/reasoning_snapshots.yaml#cognition-stability-minimalism`
- `cognition/state/drift_metrics.yaml#architectural-consistency-score`
