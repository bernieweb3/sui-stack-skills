# 2026-05-20 Operational Cognition Framework

Status: accepted
Owner: architecture
Affected layers: L0, L1, L2, L3, L4, L5, L6

## Decision

Refactor Sui Stack Skills from a knowledge repository into a lightweight operational cognition framework for long-horizon coding agents.

## Rationale

The existing repository contains useful Sui, frontend, security, and architecture knowledge, but knowledge alone does not preserve architectural intent across long sessions. Coding agents need a small persistent cognition layer that records commitments, invariants, rejected patterns, unresolved tradeoffs, and forbidden reversions before task-specific knowledge is loaded.

## Consequences

- Existing Markdown guidance is retained under `skills/`.
- L0 cognition lives in grouped YAML registries and append-only decision logs.
- Harnesses define task-local execution gates without becoming a rule engine.
- Constitutions define behavioral and cognitive constraints separately from knowledge.
- Multi-agent work synchronizes through L0 state rather than ad hoc chat memory.

## Linked State

- `cognition/state/architectural_commitments.yaml#minimum-viable-cognition`
- `cognition/state/architectural_commitments.yaml#l0-before-retrieval`
- `cognition/state/rejected_patterns.yaml#giant-policy-runtime`
- `cognition/state/forbidden_reversions.yaml#do-not-return-to-flat-knowledge-repo`
