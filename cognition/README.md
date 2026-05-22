# Internal Cognition Substrate

This folder is hidden infrastructure under skills.

Agents should not start here by default. Start with `skills/manifest.yaml` and `skills/*/SKILL.md`; load cognition only when a skill activation contract references it.

L0 records commitments that future coding agents must preserve unless an explicit decision updates or retires them. Skills explain what work is being activated; L0 records what must remain true and why.

## Skill-Triggered Load Order

When a selected skill references internal cognition, load:

1. `cognition/state/architectural_commitments.yaml`
2. `cognition/state/invariants.yaml`
3. `cognition/state/lifecycle_policy.yaml`
4. `cognition/state/cognitive_checksums.yaml`
5. `cognition/state/rejected_patterns.yaml`
6. `cognition/state/forbidden_reversions.yaml`
7. `cognition/state/unresolved_tradeoffs.yaml`
8. The most recent relevant file in `cognition/state/decision_log/`

For small tasks, load only the compressed context, invariants, and harness named by the active skill.

## Update Rule

Update L0 only when the task creates or changes architectural intent:

- new invariant discovered
- existing invariant relaxed or removed
- design pattern rejected with rationale
- tradeoff accepted but not fully resolved
- architectural decision made that future agents must remember
- reversion identified as unsafe

Do not record routine implementation details, temporary todos, or status updates.

## Stability Semantics

L0 cognition supports:

- compressed contexts for low-overhead runtime injection
- cognitive checksums for session-level coherence verification
- conflict detection for cognitive tension
- reasoning snapshots for concise tradeoff topology
- cognition weighting for compression and injection priority
- drift metrics for operational stability evaluation
- cognitive budgets for bounded runtime context
- promotion and demotion semantics for cognition mobility
- boundary isolation for role-scoped injection
- recovery flows for drift events
- human anchors for protected architectural authority
- supersession semantics for architectural evolution
- decision crystallization to prevent cognitive noise
- freshness semantics for stale cognition mitigation
- tiered invariants for proportional enforcement

The governing policy is `cognition/state/lifecycle_policy.yaml`.

Checksums are stored in `cognition/state/cognitive_checksums.yaml`. They are human-readable drift checks, not cryptographic hashes.

Compressed contexts are stored in `cognition/state/compressed_context.yaml`. They are the preferred injection surface for Gemini Flash and long sessions.

Conflicts, reasoning snapshots, and drift metrics are stored in:

- `cognition/state/conflict_registry.yaml`
- `cognition/state/reasoning_snapshots.yaml`
- `cognition/state/drift_metrics.yaml`

Resilience policy is stored in `cognition/state/resilience_policy.yaml`. It is the single place for budgets, mobility, isolation, recovery, and human anchors.

## Synchronization Rule

In multi-agent work, L0 changes are serialized through the planner or architecture agent. Domain agents may propose L0 updates, but they should not independently rewrite commitments that affect other domains.
