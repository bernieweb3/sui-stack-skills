# AGENT.md

This repository is a skills-first Sui coding-agent skill pack.

Agents should activate skills first. The cognition framework is internal support infrastructure.

## Default Loading

1. Read `skills/manifest.yaml`.
2. Select the narrowest matching skill in `skills/*/SKILL.md`.
3. Follow the skill activation contract.
4. Load hidden cognition, harnesses, constitutions, runtime profiles, and review contracts only when the skill references them.

## Skill Selection

Use these primary skills:

- `move_core`: Sui Move objects, capabilities, package safety.
- `ptb_runtime`: programmable transaction blocks and atomic flows.
- `wallet_ux`: wallet connection, signing states, transaction finality.
- `security_audit`: financial safety, audits, incident response.
- `sui_architecture`: cross-domain Sui architecture and infrastructure.
- `cognition_stability`: changes to this skill pack or hidden cognition substrate.

## Hidden Substrate

These folders are internal unless activated by a skill:

- `cognition/`
- `harnesses/`
- `constitutions/`
- `runtime/`
- `review/`
- `agent_roles/`

Do not ask users to manually orchestrate these layers. Skills activate them.

## Gemini Flash Hint

For Gemini 3.5 Flash and other fast agents:

- use `runtime/flash_context_profile.yaml`
- prefer compressed contexts over full RFC prose
- activate at most three skills by default
- preserve critical cognition, foundational invariants, and human anchors
- expand hidden cognition only when the active skill contract references it

## Invariant Rule

Never weaken foundational Sui safety, PTB atomicity, capability authority, or protected human anchors without explicit review.
