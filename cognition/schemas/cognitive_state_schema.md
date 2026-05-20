# Cognitive State Schema

The L0 state files use plain YAML with stable IDs. IDs should be short, readable, and durable across refactors.

## Shared Fields

```yaml
id: short-kebab-case-id
status: active | experimental | deprecated | superseded | rejected | archived | pruned
owner: architecture | move | frontend | backend | audit | planner
freshness: foundational | stable | refresh_required | volatile
crystallization: decision_log_required | checksum_only | do_not_persist
weight: critical | high | medium | low
scope:
  - path/or/domain
rationale: "Why this exists and what drift it prevents."
evidence:
  - "Relevant file, decision, or observed failure."
supersedes:
  - prior-id
superseded_by: replacement-id
deprecation_reason: "Required when status is deprecated."
last_reviewed: "YYYY-MM-DD"
```

Use `supersedes`, `superseded_by`, and `deprecation_reason` only when they apply. Do not fill them with placeholder values.

## Supersession Semantics

Statuses mean:

- `active`: currently governing cognition.
- `experimental`: trial cognition that cannot override active commitments or foundational invariants.
- `deprecated`: historically valid but discouraged for new work.
- `superseded`: replaced by another cognition item.
- `rejected`: intentionally avoided pattern.
- `archived`: retained for traceability, not loaded by runtime.
- `pruned`: removed from active cognition after reference and ownership checks.

State transition rules:

```text
experimental -> active | deprecated | rejected
active -> superseded | deprecated
deprecated -> superseded | rejected
deprecated -> archived
archived -> pruned
superseded -> active only through a new decision log entry
rejected -> active only through a new decision log entry and explicit rationale
```

Superseded items must include `superseded_by`. Successor items should include `supersedes` when the replacement is direct.

## Freshness Semantics

```yaml
freshness: foundational
refresh_policy: "Review quarterly or when contradicted by a decision."
```

Freshness values:

- `foundational`: rarely changes; refresh when architecture or Sui safety model changes.
- `stable`: valid across normal refactors; refresh when the owning layer changes.
- `refresh_required`: refresh before relying on it for implementation.
- `volatile`: refresh every session or before use.

## Cognition Weighting

```yaml
weight: critical
```

Weights affect persistence, pruning, compression, and injection order:

- `critical`: never omit from relevant compressed context; inject before implementation.
- `high`: preserve in route-level compressed context when relevant.
- `medium`: include only when the harness or task directly references it.
- `low`: omit from compressed context by default.

Critical and high cognition should have decision lineage. Medium cognition can remain checksum-only unless it recurs across sessions. Low cognition should not become L0 unless it affects operational usability.

## Cognitive Budgeting

Budgets keep runtime cognition bounded.

```yaml
cognition_budget:
  max_active_commitments: 7
  max_runtime_invariants: 12
  max_reasoning_snapshots: 3
  max_conflict_contexts: 2
  compression_priority:
    - foundational_truths
    - active_architecture
    - active_risks
```

Budget overflow handling:

- keep critical cognition before high, high before medium, medium before low
- keep foundational invariants before operational or cosmetic invariants
- replace long rationale with decision-log references
- move low and cosmetic cognition out of runtime injection
- split the task or escalate if the budget still cannot hold required cognition

## Promotion And Demotion

Promotion path:

```text
temporary insight -> validated pattern -> architectural commitment -> foundational invariant
```

Demotion path:

```text
stale architecture -> deprecated -> archived -> pruned
```

Promotion requires repeated operational evidence or safety-critical review. Demotion requires reference checks and, for foundational or human-anchored cognition, decision or human approval.

## Boundary Isolation

Boundary isolation limits agent context to relevant cognition.

```yaml
cognition_visibility:
  frontend-agent:
    visible:
      - transaction UX cognition
      - wallet approval and finality invariants
    hidden_by_default:
      - Move storage layout internals unless transaction semantics depend on them
```

Hidden cognition can be shared when a task crosses ownership, trust, safety, or transaction boundaries. Share IDs and one-line meanings first; expand full files only when needed.

## Drift Recovery

Recovery events:

- checksum mismatch
- invariant instability
- conflict escalation
- semantic reinterpretation
- rejected-pattern reintroduction
- human-anchor violation

Recovery severity:

- `low`: refresh checksum and compressed context
- `medium`: reload foundational cognition and resolve conflict before completion
- `high`: stop implementation and request architecture review
- `critical`: stop, rollback unstable cognition, reload human anchors, request human and audit review

## Human Cognition Anchors

Human anchors protect foundational architectural authority.

```yaml
human_anchor:
  authority: bernie_web3
  protected_commitments:
    - minimum-viable-cognition
    - sui-object-first-design
  protected_invariants:
    - ptb-atomicity-visible
  immutable_without_human_override: true
```

Agents may propose changes and flag drift. They must not weaken, prune, or supersede protected cognition without human override.

## Tiered Invariants

Invariant entries include a tier:

```yaml
tier: foundational | architectural | operational | cosmetic
```

Tier meanings:

- `foundational`: safety or trust guarantee; violation blocks completion.
- `architectural`: system structure or authority boundary; architecture review required for semantic change.
- `operational`: workflow or runtime discipline; fix or document exception.
- `cosmetic`: naming, formatting, or style; advisory unless usability is affected.

## Cognitive Checksum

Checksums are human-readable session coherence records, not cryptographic proofs.

```yaml
- id: session-or-task-id
  status: active
  owner: architecture
  created_at: "YYYY-MM-DD"
  updated_at: "YYYY-MM-DD"
  applies_to:
    - path/or/domain
  cognitive_checksum:
    compressed_context: framework-core-compressed
    reasoning_snapshot: cognition-stability-minimalism
    active_commitments:
      - minimum-viable-cognition
    active_invariants:
      - ptb-atomicity-visible
    touched_invariants:
      - capability-enforcement
    freshness_flags:
      - id: sdk-versioning
        freshness: refresh_required
        action: "Refresh before implementation."
    supersession_flags:
      - "No active contradiction detected."
    potential_conflicts:
      - "None."
    drift_metrics:
      - architectural-consistency-score
    drift_assessment: none | low | medium | high
    next_refresh_trigger: "Before final review."
```

Checksum validation asks:

- Are the active commitments still the same commitments the task began with?
- Did any touched invariant silently weaken?
- Did stale or superseded cognition influence the implementation?
- Is there a potential conflict that requires crystallization?

## Compressed Context

Compressed contexts are deterministic runtime packets:

```yaml
compressed_context:
  foundational_truths:
    - id: ptb-atomicity-visible
      meaning: "Dependent Sui operations must state their PTB atomicity boundary."
  active_architecture:
    - id: skills-are-knowledge-not-policy
      meaning: "Skills teach; L0 and harnesses constrain."
  active_risks:
    - id: shared-object-contention
      meaning: "Shared object throughput can introduce serialization."
  forbidden_regressions:
    - id: giant-policy-runtime
      meaning: "Do not introduce an autonomous rule engine."
  expansion_refs:
    - cognition/state/invariants.yaml
```

Compression rules:

- preserve critical cognition and foundational invariants
- preserve forbidden regressions relevant to the route
- compress by IDs and one-line meanings
- expand source refs only when the task touches that topic
- do not include long historical rationale by default

## Conflict Detection

Conflict records stay small:

```yaml
- id: object-ownership-vs-shared-object-throughput
  status: active
  type: soft_tension
  severity: medium
  commitment_a: sui-object-first-design
  commitment_b: shared-object-parallelism
  resolution_required: true
```

Conflict types:

- `hard_contradiction`: active items cannot both govern the task.
- `soft_tension`: coexistence is possible with explicit tradeoff handling.
- `semantic_ambiguity`: wording can be interpreted in incompatible ways.
- `drift_signal`: implementation weakens or reinterprets active cognition.

Severity:

- `low`: checksum note is enough.
- `medium`: resolve or document before completion.
- `high`: architecture review.
- `critical`: architecture and audit review.

## Reasoning Snapshot

Snapshots preserve reasoning shape, not verbose reasoning transcripts.

```yaml
reasoning_snapshot:
  problem: "shared treasury scalability"
  considered_options:
    - shared_objects
    - party_objects
  dominant_constraints:
    - PTB contention
    - UX simplicity
  rejected_alternatives:
    - global shared treasury object
  selected_tradeoff: "optimize for single-user latency"
  trust_assumptions:
    - "No backend custody."
  protocol_assumptions:
    - "PTB boundary remains explicit."
```

Create snapshots only when a tradeoff shape needs to survive future sessions.

## Drift Metrics

Drift metrics are session and review signals, not academic benchmarks.

```yaml
- id: architectural-consistency-score
  category: retention
  measurement: "100 minus weighted penalties for unresolved conflicts, weakened invariants, stale cognition reliance, and rejected-pattern reintroduction."
  update_frequency: per_session
  target: ">= 90 before completion."
```

Useful metric categories:

- retention
- conflict
- reinterpretation
- safety
- entropy
- compression

## Decision Crystallization

Crystallize only cognition that future agents must preserve.

Crystallize:

- architecture decisions
- invariant changes
- ownership model changes
- trust boundary changes
- protocol assumptions
- security assumptions
- PTB semantics
- authority boundaries

Do not crystallize:

- naming tweaks
- cosmetic refactors
- local cleanup
- formatting changes
- minor implementation details

Persistence thresholds:

- `decision_log_required`: semantic L0 change or authority/trust/invariant boundary.
- `checksum_only`: session coherence note without lasting architecture change.
- `do_not_persist`: no L0 persistence; keep the change local to normal code/docs review.

## Decision Log Entry

Decision logs are append-only Markdown files under `cognition/state/decision_log/`.

```markdown
# YYYY-MM-DD Short Decision Title

Status: accepted
Owner: architecture
Affected layers: L0, L2, L3

## Decision

Concrete decision in one paragraph.

## Rationale

Why this decision is necessary and what failure mode it prevents.

## Consequences

- What becomes easier.
- What becomes constrained.
- What future agents must not reverse casually.

## Linked State

- `architectural_commitments.yaml#commitment-id`
- `invariants.yaml#invariant-id`
```

## Review Discipline

Every change to an active commitment, invariant, rejected pattern, or forbidden reversion should add or update a decision log entry. The decision log is the source of rationale; the YAML registries are the runtime surface.
