---
name: system-architecture
description: "Design or critique system boundaries, data shapes, ownership, lifecycle, concurrency, trust, and failure semantics before implementation. Use for service boundaries, adapters, daemons, agents, providers, integrations, and cross-cutting features."
---

# System architecture

Architecture is a set of ownership and boundary decisions that make behavior easier to prove.

## Start with the shape of the system

Before naming classes or files, write down:

- actors and processes,
- authoritative state,
- derived/cached state,
- commands and events,
- external inputs,
- side effects,
- trust boundaries,
- retry/cancellation boundaries,
- persistence boundaries.

If the design cannot explain who owns a piece of mutable state, it is not ready.

## Boundary test

For every proposed boundary, answer:

1. What changes independently on each side?
2. What contract crosses the boundary?
3. What validation happens at entry?
4. Who owns retries and timeouts?
5. Who owns credentials?
6. Who owns persistence?
7. What happens if either side restarts halfway through?
8. How is success reconciled rather than merely assumed?

Reject a boundary that only adds indirection without isolating ownership, volatility, trust, or lifecycle.

## Adapter discipline

Use adapters when they isolate a genuinely variable external system.

Common useful shapes include:

- `PlatformAdapter`: OS/process/filesystem differences.
- `HostAdapter`: application-specific integration behavior.
- `ProviderAdapter`: model/API/provider protocol differences.
- `VenueAdapter`: exchange/market venue differences.
- `StorageAdapter`: persistence engine differences.

Keep domain decisions outside adapters. An adapter translates contracts; it should not quietly become the business-logic layer.

## Data modeling

Prefer structures that make invalid states difficult to represent:

- explicit enums/state machines for lifecycle,
- typed IDs rather than ambiguous strings,
- immutable event records for evidence/provenance,
- registries/tables when behavior varies by known kind,
- separate intent from observed outcome,
- explicit timestamps and source identity on externally derived data.

For financial/execution systems, model `intent`, `risk decision`, `submission`, `acknowledgement`, `fill`, `reconciliation`, and `review` as distinct states.

For agent systems, model `request`, `plan`, `delegation`, `tool call`, `result`, `memory write`, `verification`, and `final state` separately when those distinctions matter operationally.

## Concurrency

Identify every actor that can write the same state.

Prefer eliminating shared mutation over serializing it with a lock. If shared state remains, specify:

- ownership,
- atomicity boundary,
- idempotency key,
- conflict behavior,
- crash recovery,
- ordering guarantees.

## Lifecycle

Design startup, steady state, restart, reconnect, partial failure, upgrade, and shutdown.

A system that only has a happy-path request diagram is incomplete.

For daemons/watchers, define:

- singleton semantics,
- stale process detection,
- port/socket ownership,
- health vs readiness,
- watcher duplication prevention,
- cleanup after crashes,
- persisted state recovery.

## Trust and secrets

Credentials cross as few boundaries as possible.

- Parse and validate untrusted data at the edge.
- Do not persist secrets in logs or evidence bundles.
- Give workers only the capabilities required for their task.
- Separate read-only observation from mutation permissions.

## Design output

Return:

1. **Context**: current constraints and existing boundaries.
2. **Data model**: core records/state transitions.
3. **Boundary map**: components and contracts.
4. **Lifecycle**: startup through recovery/shutdown.
5. **Failure model**: what can fail and who handles it.
6. **Trust model**: secrets, permissions, untrusted inputs.
7. **Alternatives considered**: at least two when the decision is novel or costly to reverse.
8. **Verification plan**: how a reviewer can prove the architecture behaves as designed.
9. **Decision**: smallest design that satisfies the constraints.
