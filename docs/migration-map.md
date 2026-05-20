# Migration Map

This document explains how the original Sui Stack Skills repository was refactored into the Sui Stack Operational Cognition Framework.

## Folder Mapping

| Original folder | New location | New role |
| --- | --- | --- |
| `sui-stack-development/` | `skills/domain/sui/sui-stack-development/` | Sui Move and CLI domain skills |
| `sui-infrastructure/` | `skills/domain/infrastructure/sui-infrastructure/` | Sui infrastructure domain skills |
| `frontend-development/` | `skills/implementation/frontend/frontend-development/` | Frontend implementation skills |
| `common-skills/` | `skills/implementation/engineering/common-skills/` | General implementation skills |
| `preparation-before-project/` | `skills/architecture/project-preparation/preparation-before-project/` | Product and project architecture skills |
| `system-design/` | `skills/architecture/system-design/system-design/` | Architecture reference skills |
| `advanced-security-and-optimization/` | `skills/review/security-optimization/advanced-security-and-optimization/` | Review and optimization skills |

## Decomposition Strategy

The original repository mixed knowledge, behavior, and review expectations inside skill prose. The new layout separates them:

- Knowledge stays in `skills/`.
- Persistent intent moves to `cognition/state/`.
- Task execution gates move to `harnesses/`.
- Reasoning constraints move to `constitutions/`.
- Completion checks move to `review/`.
- Multi-agent authority boundaries move to `agent_roles/`.
- Loading and synchronization rules move to `runtime/`.

## What Remains a Skill

Keep a file as a skill when it answers:

- How do I implement this domain pattern?
- What APIs, frameworks, or language features should I use?
- What examples help me produce code?
- What are the practical best practices for this topic?

Examples:

- Sui Move smart contract development
- Sui CLI usage
- dApp Kit setup
- React, Next.js, Vue, Tailwind guidance
- DeFi architecture reference

## What Becomes Cognitive State

Move content into L0 when it answers:

- What architectural commitment must future agents preserve?
- What invariant must survive refactors?
- What pattern was rejected and why?
- What unresolved tradeoff should remain visible?
- What reversion is forbidden?

Examples:

- "Skills are knowledge, not policy."
- "Capability-first Move access control is active."
- "Do not return to a flat knowledge-only repository."

Use `cognition/state/lifecycle_policy.yaml` to decide whether the item becomes:

- an active commitment
- a tiered invariant
- a compressed context item
- a conflict record
- a reasoning snapshot
- a rejected pattern
- a freshness flag
- a checksum-only session note
- a drift metric signal
- a resilience policy rule
- a decision log entry

Do not crystallize routine implementation details.

## What Becomes a Harness

Move content into a harness when it answers:

- What must an agent load before this task?
- What must be checked before, during, and after execution?
- What patterns are forbidden for this task type?
- What review is required?

Examples:

- Move harness
- PTB harness
- Frontend transaction harness
- Financial safety harness

## What Becomes a Constitution

Move content into a constitution when it constrains reasoning:

- Load L0 before broad retrieval.
- Do not reverse a decision without rationale.
- Reason from Sui object ownership before account assumptions.
- Refresh active invariants before large edits.

## What Should Not Exist Yet

Do not build these in the MVP:

- autonomous orchestration daemon
- custom DSL for rules
- graph database for cognition
- one-file-per-rule YAML explosion
- automated policy interpreter
- distributed multi-agent runtime
- symbolic reasoning engine
- cryptographic checksum machinery
- automated supersession ontology
- memory pruning daemon

These may become useful later only if repeated real workflows prove that Markdown and grouped YAML cannot reduce drift enough.
