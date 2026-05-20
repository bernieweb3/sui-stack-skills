# Minimum Viable Runtime

The runtime is a manual, lightweight loading and update protocol. It does not require a daemon, policy engine, vector database, or orchestration service.

## Before Task

1. Classify the task using `runtime/task_router.yaml`.
2. Select the compressed context named by the route.
3. Select the role visibility scope and cognition budget.
4. Load relevant L0 state, including lifecycle policy, resilience policy, active checksum, conflict registry, and drift metrics.
5. Check supersession status, freshness, cognition weights, human anchors, and budget fit.
6. Load one primary harness.
7. Load only the skills named by that harness.
8. Load the core constitution and any domain constitution named by the harness.
9. For Gemini Flash, use `runtime/flash_context_profile.yaml` to keep the packet small.

## During Task

1. Name the active invariants at risk.
2. Name the invariant tier: foundational, architectural, operational, or cosmetic.
3. Name the cognition weight: critical, high, medium, or low.
4. Keep the active cognition set within budget.
5. Keep implementation within the harness execution gates.
6. Update the task checksum when commitments, invariants, freshness, conflicts, anchors, or compressed context coverage change.
7. Promote, demote, archive, or prune cognition only through the mobility rules.
8. Create a reasoning snapshot only when a real architectural tradeoff must be preserved.
9. Escalate if a change touches active L0 commitments, forbidden reversions, foundational invariants, critical cognition, human anchors, or cross-domain ownership.
10. Trigger drift recovery if checksum mismatch, invariant instability, semantic reinterpretation, rejected-pattern reintroduction, or anchor violation appears.
11. Refresh L0 after long context accumulation, before large boundary edits, or when freshness is `refresh_required` or `volatile`.

## After Task

1. Run relevant verification.
2. Validate the checksum against changed files and review contracts.
3. Check open conflicts and drift metrics.
4. Validate budget, visibility scope, and human-anchor rules.
5. Decide whether the result should crystallize.
6. Update L0 only if architectural intent changed.
7. Add a decision log entry when commitments, invariants, rejected patterns, forbidden reversions, conflict severity, mobility status, anchor state, or supersession state change.
8. Ask for review according to the active harness.

## Synchronization Model

Single-agent work updates L0 directly when the agent is acting as architecture owner.

Multi-agent work serializes L0 changes through the planner or architecture agent. Domain agents propose updates and include rationale; the architecture agent merges or rejects them.

## Runtime Validation

Before completion, validate:

- no active item conflicts with another active item
- no superseded item is being applied as current guidance
- no rejected pattern was reintroduced
- foundational invariants were not weakened
- critical cognition survived compression
- `refresh_required` cognition was refreshed before implementation
- checksum drift is `none` or `low`
- no medium-or-higher conflict remains unresolved
- architectural consistency score is at least 90 unless the review explicitly accepts the risk
- cognition budget was not exceeded without architecture review
- role visibility did not hide cognition required for safety or architecture
- human-anchored cognition was not weakened without human override
- recovery flow completed for any medium-or-higher drift event
- crystallized decisions have decision log rationale

## Compression Flow

1. Start with the route's compressed context.
2. Expand only source files referenced by `expansion_refs` when the task touches that topic.
3. Preserve all critical cognition and all foundational invariants.
4. Omit low-weight cosmetic cognition unless the task is about docs or formatting.
5. If compression hides a task-critical item, update `compression-effectiveness` in `drift_metrics.yaml`.

## Budgeting Flow

1. Use the budget named by the route, usually `gemini_flash` for Flash-class agents.
2. Keep critical cognition, foundational invariants, and relevant human anchors first.
3. Drop low-weight and cosmetic cognition from runtime injection before dropping anything else.
4. Replace long rationale with decision-log references.
5. If still over budget, split the task or request architecture review.

## Boundary Isolation Flow

1. Use the route visibility scope from `resilience_policy.yaml`.
2. Inject IDs and one-line meanings before whole files.
3. Share hidden cognition only when a task crosses ownership, trust, safety, or transaction boundaries.
4. Let architecture mediate cross-boundary conflicts and audit request any safety-relevant cognition.

## Drift Recovery Flow

1. Classify the drift event: checksum mismatch, invariant instability, conflict escalation, semantic reinterpretation, rejected-pattern reintroduction, or anchor violation.
2. Apply the severity recovery from `resilience_policy.yaml`.
3. Reload foundational cognition and compressed context for medium or higher events.
4. Stop and request human/audit review for critical anchor, trust, fund-safety, or PTB violations.
