# hstack

`hstack` is an evidence-first engineering skills package for building and operating complex software with AI agents.

It is tuned for work where correctness depends on understanding a real codebase, crossing process or service boundaries, inspecting live runtime behavior, and producing verification that another engineer can rerun.

## What it is for

- agent runtimes, orchestration systems, MCP/tooling, memory and provenance layers
- trading, market-data, execution, risk, and on-chain infrastructure
- large repository archaeology and clean-room architecture studies
- Rust, TypeScript, Python, services, daemons, CLIs, Electron/browser surfaces
- Windows-first development with WSL/Linux parity checks
- local verification when hosted CI is unavailable or intentionally not used
- long-running and multi-agent work that must leave an auditable trail

## Core rule

**Do not confuse generated output with proven behavior.**

Every non-trivial task should move through five states:

1. **Understand** the existing system and boundaries.
2. **Model** the intended behavior and failure modes.
3. **Change** the smallest coherent unit.
4. **Verify** against the real artifact or runtime.
5. **Record** evidence, decisions, and remaining uncertainty.

## Skills

| Skill | Purpose |
|---|---|
| `hstack-mode` | Default router for rigorous work. Chooses the right workflow and enforces evidence gates. |
| `repo-archaeology` | Builds a source-grounded map of an unfamiliar repository before proposing changes. |
| `system-architecture` | Designs boundaries, data shapes, ownership, lifecycle, trust, and failure semantics before implementation. |
| `runtime-forensics` | Diagnoses live behavior from logs, processes, sockets, traces, state, and reproducible symptoms. |
| `local-verification` | Creates reproducible local verification on Windows and WSL/Linux without assuming GitHub Actions. |
| `agentic-systems-review` | Reviews agents, tools, orchestration, memory, permissions, retries, and provenance. |
| `market-systems-review` | Reviews market-data, signals, risk, execution, venue adapters, reconciliation, and failure containment. |
| `adversarial-review` | Uses independent lenses to try to break a diff, design, or operational claim. |
| `handoff` | Produces a compact continuation record with exact state, evidence, and next executable step. |

## Subagents

- `fresh-eyes-review`: independent skeptical reviewer.
- `runtime-investigator`: read-only runtime and evidence investigator.
- `architecture-critic`: boundary, state, concurrency, and lifecycle critic.

## Recommended flow

For most substantial tasks, start with `hstack-mode`.

Typical routes:

- unfamiliar repo → `repo-archaeology` → `system-architecture`
- bug → reproduce → `runtime-forensics` → smallest fix → `local-verification` → `adversarial-review`
- agent platform → `repo-archaeology` → `agentic-systems-review` → `system-architecture` → verify
- trading system → `repo-archaeology` → `market-systems-review` → runtime/replay verification
- large multi-session effort → map decisions first, then execute one verifiable unit at a time and finish each session with `handoff`

## Safety and Git discipline

Default to additive, reviewable work.

- Never force-push, merge, deploy, delete production data, rotate real credentials, or send customer/user communications without explicit authorization.
- Do not push directly to a protected/default branch when a branch and PR can represent the work.
- Never log secrets. Treat credentials, wallet keys, tokens, cookies, signing material, and exchange/API secrets as boundary data.
- Separate observation from mutation. Investigation workflows are read-only unless the task explicitly requires a change.
- A green build is evidence, not proof of runtime behavior.

## Provenance

`hstack` is a clean synthesis inspired by patterns in Matt Pocock's Skills for Real Engineers and Cursor's `pstack`, then specialized for Havokyn-style engineering work. See `THIRD_PARTY.md`.
