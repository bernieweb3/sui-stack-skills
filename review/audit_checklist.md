# Audit Checklist

Use this checklist when a task touches Move modules, PTBs, financial flows, wallet signing, or framework-level cognition.

## L0 Checks

- Active commitments still hold.
- Active invariants still hold.
- Human cognition anchors are preserved.
- Cognition budget did not omit safety-critical context.
- Agent boundary isolation did not hide audit-relevant cognition.
- Invariant tiers were respected.
- Critical and high-weight cognition survived compression.
- Freshness flags were refreshed when required.
- Medium-or-higher conflicts were resolved or escalated.
- Superseded or deprecated cognition was not used as current authority.
- Rejected patterns were not reintroduced.
- Forbidden reversions were not performed.
- Cognitive checksum matches the work performed.
- Drift metrics reflect real drift signals, not cosmetic noise.
- Drift recovery was completed for any medium-or-higher event.
- New architectural decisions have decision log entries.

## Move Checks

- Object ownership model is explicit.
- Capability authority is explicit.
- Shared object usage has a contention rationale.
- Public and entry surfaces are intentional.
- Upgrade and pause authority are known.

## PTB Checks

- Atomic command sequence is documented.
- Signer, sponsor, gas, and approval surfaces are clear.
- Dry-run and failure behavior is clear.
- Transaction digest/finality behavior is clear.

## Financial Checks

- Value conservation is checked.
- Oracle freshness and deviation are checked.
- Slippage limits are checked.
- Pause and emergency response paths are preserved.
- Optimizations do not weaken safety assumptions.
