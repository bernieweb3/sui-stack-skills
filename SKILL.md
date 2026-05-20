# Sui Stack Operational Cognition Framework

> Author: Bernie Nguyen (BernieWeb3)  
> License: Apache License 2.0  
> Version: 2.0.0

This repository is no longer only a Sui developer knowledge base.

It is a lightweight operational cognition framework for coding agents working on Sui, Move, PTB-heavy systems, frontend transaction flows, and long-horizon software engineering tasks.

The success metric is simple:

> reduce architectural drift across long coding sessions.

## Operating Model

```text
L0  cognition/      Persistent architectural cognition
L1  skills/         Domain and implementation knowledge
L2  runtime/        Context routing and loading protocol
L3  harnesses/      Task-local execution gates
L4  constitutions/  Behavioral and cognitive constraints
L5  review/         Completion and audit contracts
L6  agent_roles/    Multi-agent ownership and escalation
```

## Start Here

For framework design and usage:

- [Operational Cognition RFC](docs/operational-cognition-rfc.md)
- [Migration Map](docs/migration-map.md)

For persistent cognition:

- [L0 Cognitive State](cognition/README.md)
- [Architectural Commitments](cognition/state/architectural_commitments.yaml)
- [Invariants](cognition/state/invariants.yaml)
- [Lifecycle Policy](cognition/state/lifecycle_policy.yaml)
- [Resilience Policy](cognition/state/resilience_policy.yaml)
- [Compressed Context](cognition/state/compressed_context.yaml)
- [Cognitive Checksums](cognition/state/cognitive_checksums.yaml)
- [Conflict Registry](cognition/state/conflict_registry.yaml)
- [Reasoning Snapshots](cognition/state/reasoning_snapshots.yaml)
- [Drift Metrics](cognition/state/drift_metrics.yaml)
- [Rejected Patterns](cognition/state/rejected_patterns.yaml)
- [Forbidden Reversions](cognition/state/forbidden_reversions.yaml)

For task execution:

- [Runtime Model](runtime/README.md)
- [Task Router](runtime/task_router.yaml)
- [Gemini Flash Context Profile](runtime/flash_context_profile.yaml)
- [Move Harness](harnesses/move.yaml)
- [PTB Harness](harnesses/ptb.yaml)
- [Frontend Transaction Harness](harnesses/frontend-transaction.yaml)
- [Financial Safety Harness](harnesses/financial-safety.yaml)

## Runtime Flow

Before a substantial task:

1. Route the task using `runtime/task_router.yaml`.
2. Select the compressed context.
3. Select the role visibility scope and cognition budget.
4. Load relevant L0 cognitive state.
5. Check lifecycle status, freshness, supersession, cognition weight, and human anchors.
6. Load or create the cognitive checksum.
7. Load one primary harness.
8. Load only the skills named by that harness.
9. Load the core constitution and any domain constitution.

During the task:

1. Preserve active invariants.
2. Preserve architectural commitments.
3. Track touched invariant tiers.
4. Preserve critical and high-weight cognition during compression.
5. Keep the active cognition set within budget.
6. Refresh stale cognition before relying on it.
7. Record conflicts and drift signals when they affect implementation.
8. Run recovery for medium-or-higher drift events.
9. Avoid rejected patterns.
10. Escalate if authority boundaries or human anchors are crossed.

After the task:

1. Run verification.
2. Apply review contracts.
3. Validate checksum drift is `none` or `low`.
4. Confirm no unresolved medium-or-higher conflict remains.
5. Confirm human anchors were not weakened.
6. Update drift metrics only for real drift signals.
7. Crystallize only architecturally meaningful cognition.
8. Update L0 only if architectural intent changed.
9. Add a decision log entry for any L0 state change.

## Skills

The original knowledge base remains under `skills/`:

- `skills/domain/sui/`
- `skills/domain/infrastructure/`
- `skills/implementation/frontend/`
- `skills/implementation/engineering/`
- `skills/architecture/`
- `skills/review/`

Skills are knowledge. They are not hidden policy. Mandatory behavior belongs in cognition, harnesses, constitutions, or review contracts.

## Gemini 3.5 Flash Usage

For Flash-class models, keep context narrow:

1. Task objective
2. compressed context
3. cognition budget and human-anchor flags
4. active checksum
5. relevant L0 IDs with invariant tiers and weights
6. freshness, conflict, recovery, and supersession flags
7. one harness
8. one constitution
9. narrow skill excerpts

Refresh L0 before large edits, after task pivots, and before final review.

## What This Repository Does Not Build

This MVP intentionally avoids:

- autonomous orchestration engines
- symbolic cognition graphs
- giant policy runtimes
- custom DSLs
- excessive YAML fragmentation
- distributed cognition theory

The framework stays useful only if it remains readable, composable, and easy for coding agents to actually follow.
