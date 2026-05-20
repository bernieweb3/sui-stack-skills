# Skills Layer

Skills are the L1 knowledge layer. They teach domain patterns, implementation techniques, and architectural references.

Skills should not carry hidden mandatory policy. If a rule must constrain future behavior, put it in:

- `cognition/state/` for persistent architectural intent
- `harnesses/` for task-local execution gates
- `constitutions/` for reasoning constraints
- `review/` for completion contracts

## Current Taxonomy

```text
skills/
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
