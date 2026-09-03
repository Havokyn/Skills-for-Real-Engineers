---
name: hstack-mode
description: "Default evidence-first workflow router for non-trivial engineering work. Use when a task spans architecture, implementation, runtime behavior, agents, infrastructure, or high-confidence verification."
disable-model-invocation: true
mode: true
---

# hstack mode

Use this mode when the cost of being plausibly wrong is higher than the cost of inspecting the system.

## Operating contract

For every non-trivial task, maintain these invariants:

1. **Source before story.** Inspect the repository, runtime, docs, history, or external contract before explaining how it works.
2. **State the boundary.** Name the process, service, module, adapter, trust boundary, and persistence boundary relevant to the change.
3. **Name the data shape.** Identify the records, events, commands, state machines, registries, or typed models that carry behavior.
4. **Separate observation from mutation.** Investigation is read-only. Changes begin only after the current behavior is grounded.
5. **Prefer the smallest coherent change.** Do not create an abstraction until the boundary or repeated behavior earns it.
6. **Verify the real thing.** Compilation, type checking, and unit tests are useful gates. They do not substitute for runtime or artifact proof when behavior is observable.
7. **Preserve evidence.** Record exact commands, refs, SHAs, logs, ports, paths, test cases, inputs, outputs, and remaining uncertainty needed to reproduce the conclusion.
8. **Protect irreversible state.** Never force-push, merge, deploy, delete persistent/production data, rotate live credentials, sign transactions, or send external communications without explicit authority.

## Route the task

Choose the narrowest workflow that covers the task.

### Investigation

Use for "how does this work?", "what is actually happening?", unfamiliar repositories, clean-room studies, and architecture decomposition.

1. Run `repo-archaeology`.
2. Trace the requested path end to end with exact files/functions where available.
3. If the question crosses processes or adapters, run `system-architecture` as a read-only mapping pass.
4. Report facts, inferred behavior, and unknowns separately.

### Bug or runtime symptom

1. Reproduce or capture the symptom before editing code.
2. Run `runtime-forensics`.
3. Identify the first violated invariant or incorrect state transition.
4. Make the smallest root-cause fix.
5. Run `local-verification` against the same surface that reproduced the problem.
6. Run `adversarial-review` before declaring done.

### Feature or integration

1. Run `repo-archaeology` on the touched subsystem if it is not already understood.
2. Run `system-architecture` before code when the feature crosses a function, process, persistence, provider, or trust boundary.
3. Define acceptance evidence before implementation.
4. Implement in verifiable units.
5. Run the relevant domain review: `agentic-systems-review` or `market-systems-review`.
6. Run `local-verification`, then `adversarial-review`.

### Agentic system

Use for agent runtimes, MCP/tool systems, model routing, orchestration, memory, provenance, permissions, and autonomous workflows.

1. Map controller, worker, tool, model, memory, queue, and persistence boundaries.
2. Run `agentic-systems-review`.
3. Prove cancellation, retry, idempotency, permission, and provenance behavior where applicable.
4. Verify a real task end to end, not only individual tool calls.

### Market or trading system

Use for market data, scanning, signals, risk, exchange/venue adapters, execution, reconciliation, and on-chain systems.

1. Map data source -> normalization -> decision -> risk -> order intent -> venue -> fill -> reconciliation -> record.
2. Run `market-systems-review`.
3. Distinguish simulated, delayed, fixture, testnet, paper, and live data explicitly.
4. Never treat a successful request as proof of correct execution state. Reconcile downstream state.

### Large or multi-session effort

1. Define the destination and non-goals.
2. Split unknowns from implementation tasks.
3. Resolve decision-blocking unknowns before opening broad implementation fronts.
4. Give each parallel worker isolated ownership where possible. Avoid shared mutable branches/files/state.
5. End every session with `handoff`.

## Parallelism rule

Parallel work is allowed when workers have distinct ownership or are intentionally independent attempts.

Use parallelism for:

- independent repository slices,
- competing architecture proposals,
- separate review lenses,
- venue/provider adapters with a shared contract,
- read-only evidence collection.

Do not parallelize writes into the same stateful surface merely to increase throughput.

## Completion gate

Do not say "done" unless you can answer all five:

- What changed or what was learned?
- What evidence supports it?
- What real surface was verified?
- What remains uncertain or unverified?
- What exact ref/path/state should the next engineer start from?
