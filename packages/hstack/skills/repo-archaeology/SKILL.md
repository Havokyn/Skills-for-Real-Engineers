---
name: repo-archaeology
description: "Build a source-grounded architecture and execution map of an unfamiliar or inherited repository before proposing changes. Use for deep dives, clean-room studies, forks, migrations, or 'how does this actually work?' questions."
---

# Repository archaeology

The deliverable is an evidence-backed map of the existing system, not a rewrite proposal.

## Rules

- Read before editing.
- Prefer manifests, entry points, tests, scripts, adapters, persistence code, and packaging/runtime configuration over README claims.
- Treat generated files and docs as secondary evidence unless runtime behavior confirms them.
- Preserve exact paths and symbol names in the final trace.
- When behavior is inferred rather than directly observed, label it as inference.

## Pass 1: establish the repository frame

Capture:

- current branch/ref and relevant commit SHA,
- top-level packages/workspaces/crates/services,
- build and package managers,
- executable entry points,
- test entry points,
- configuration and environment surfaces,
- persistence locations,
- network/process boundaries,
- external providers and protocols,
- release/packaging path.

If multiple repositories participate, map each independently before drawing cross-repo edges.

## Pass 2: map ownership and boundaries

For each major component, record:

- responsibility,
- public interface,
- inputs and outputs,
- owned state,
- dependencies,
- callers,
- lifecycle,
- failure surface.

Pay special attention to adapters. A platform adapter, host/app adapter, provider adapter, exchange adapter, storage adapter, or model adapter is a boundary claim. Verify what it actually isolates.

## Pass 3: trace execution

For each requested flow, trace from trigger to terminal effect.

A good trace names:

1. trigger or entry point,
2. function/module transitions,
3. process or network hops,
4. state reads/writes,
5. external calls,
6. retries/watchers/background loops,
7. terminal output or side effect.

Do not skip lifecycle code such as startup, shutdown, recovery, watcher registration, reconnection, migrations, or cleanup.

## Pass 4: triangulate with tests and history

Use tests to answer what the authors intended to remain invariant.

Use Git history, issues, and PRs when available to distinguish:

- deliberate architecture,
- compatibility scaffolding,
- accidental complexity,
- dead or transitional code.

Do not let history override current runtime reality.

## Pass 5: produce the map

Output these sections:

### Repository map

Packages/modules and dependency edges.

### Process map

Which components share a process and which communicate across IPC, HTTP, WebSocket, stdio, queues, files, database connections, browser/CDP, or other boundaries.

### State map

What persists, where it persists, who owns writes, and how state is recovered.

### Execution traces

Exact file/function traces for the flows requested by the user.

### Trust and credential boundaries

Where untrusted input becomes trusted, where credentials enter, and which components can execute commands or mutate external state.

### Proven facts vs inferences

Separate directly observed evidence from conclusions that still need runtime validation.

### Architectural pressure points

Only after mapping the current system, list areas where ownership, state, coupling, or lifecycle behavior creates risk. Do not propose a rewrite unless the task asks for one.
