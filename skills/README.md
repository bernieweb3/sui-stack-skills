# Skills

Skills are the primary interface of this repository.

Use first-class skills before browsing the legacy knowledge taxonomy:

- `move_core`
- `ptb_runtime`
- `wallet_ux`
- `security_audit`
- `sui_architecture`
- `cognition_stability`

Each first-class skill has a `SKILL.md` activation contract. That contract loads the hidden cognition substrate, harnesses, constitutions, runtime profile, and review contract.

## Manifest Files

- `skills/manifest.yaml`
- `skills/activation.schema.yaml`

## Knowledge Taxonomy

```text
skills/
  move_core/
  ptb_runtime/
  wallet_ux/
  security_audit/
  sui_architecture/
  cognition_stability/
  architecture/
    project-preparation/
    system-design/
  domain/
    infrastructure/
    sui/
  implementation/
    engineering/
    frontend/
  review/
    security-optimization/
```

## Naming

- Keep original Markdown filenames when migrating existing skills.
- Use lowercase hyphenated directories.
- Use one skill file per practical domain topic.
- Do not split a skill unless agents routinely need only part of it.

## Boundary Rule

Skills are user-facing. Cognition, harnesses, constitutions, runtime, review, and agent roles are internal unless activated by a skill contract.
