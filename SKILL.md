# Sui Stack Skills

> Skills-first operational cognition for Sui, Move, PTB, wallet UX, and long-horizon coding agents.  
> Author: Bernie Nguyen (BernieWeb3)  
> License: Apache-2.0  
> Version: 3.0.0

This repository is a filesystem-native skill pack.

The primary abstraction is:

```yaml
active_skills:
  - move_core
  - ptb_runtime
  - wallet_ux
```

The cognition framework is hidden infrastructure under skills. Users and agents should start with skills, not with cognition layers.

## Install Shape

```bash
npx skills add bernieweb3/sui-stack-skills
```

Equivalent loaders can read:

- `SKILL.md`
- `AGENT.md`
- `skills/manifest.yaml`
- `skills/*/SKILL.md`

## Primary Skills

| Skill | Use when |
| --- | --- |
| `move_core` | Sui Move objects, capabilities, package safety, shared-object decisions |
| `ptb_runtime` | Programmable transaction blocks, atomic flows, sponsorship, dry-runs |
| `wallet_ux` | Wallet connection, signing states, transaction finality, dApp Kit UX |
| `security_audit` | Financial safety, authorization, audits, incident response |
| `sui_architecture` | Cross-domain Sui architecture, infrastructure, system design |
| `cognition_stability` | Maintaining this skill pack or hidden cognition substrate |

Skill entrypoints:

- [Move Core](skills/move_core/SKILL.md)
- [PTB Runtime](skills/ptb_runtime/SKILL.md)
- [Wallet UX](skills/wallet_ux/SKILL.md)
- [Security Audit](skills/security_audit/SKILL.md)
- [Sui Architecture](skills/sui_architecture/SKILL.md)
- [Cognition Stability](skills/cognition_stability/SKILL.md)

## Skill Activation

Each skill is a cognition activation contract. It declares:

- cognition to load
- invariants to preserve
- harnesses to apply
- constitutions to follow
- review contracts to check
- runtime profile and budget
- model-specific compression preferences

Schema:

- [Activation Schema](skills/activation.schema.yaml)
- [Skill Manifest](skills/manifest.yaml)

## Hidden Infrastructure

These folders support skill activation but are not the primary UX:

- `cognition/`
- `harnesses/`
- `constitutions/`
- `runtime/`
- `review/`
- `agent_roles/`

Agents should load these only when a selected skill references them.

## Agent Loading

For Claude, Antigravity, Codex, OpenCode/OpenClaw, and AGENT.md-style systems:

1. Read [AGENT.md](AGENT.md).
2. Read [skills/manifest.yaml](skills/manifest.yaml).
3. Select the narrowest matching skill.
4. Follow that skill's activation contract.
5. Use hidden cognition only through the skill contract.

## Gemini 3.5 Flash

Gemini Flash should use skills with compressed cognition:

- activate at most three skills by default
- prefer compressed contexts over full RFC prose
- preserve critical cognition and foundational invariants
- keep human anchors visible when relevant
- expand hidden cognition only when the skill contract requires it

## Design Rule

Skills are the interface. Cognition is the substrate.

Do not turn this repository into an executable orchestration system, policy engine, theorem prover, or governance runtime.
