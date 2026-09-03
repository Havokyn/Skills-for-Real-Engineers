---
name: runtime-forensics
description: "Diagnose live software behavior from reproducible symptoms and runtime evidence. Use for daemons, watchers, ports, processes, queues, Electron/CDP, services, agents, memory leaks, reconnect loops, and 'works in code but not in reality' failures."
---

# Runtime forensics

Do not patch a runtime symptom before proving where the observed state diverges from the intended state.

## 1. Freeze the claim

Write the symptom as an observable statement:

- expected behavior,
- actual behavior,
- environment,
- exact trigger,
- time window,
- affected process/account/provider/venue,
- whether it is deterministic or intermittent.

Avoid labels such as "race condition" or "network issue" until evidence supports them.

## 2. Capture the runtime topology

Record the relevant:

- processes and parent/child relationships,
- executable paths and versions,
- ports/sockets/pipes,
- environment/config sources,
- working directories,
- persistent state paths,
- background tasks/watchers,
- network endpoints,
- active sessions/connections.

For Electron/browser integration, include remote-debugging port ownership, CDP targets, renderer identity, injected assets, reconnect behavior, and host restarts.

## 3. Reproduce on the real surface

Use the same executable, protocol, account mode, provider, and state transition implicated by the report whenever safe.

A unit test that simulates the symptom can become a regression test later. It is not the initial reproduction if the failure only exists across real processes or external boundaries.

## 4. Build a timeline

Correlate timestamps across logs, events, process starts, requests, retries, state writes, and external acknowledgements.

Look for the first divergence, not the loudest downstream error.

Useful questions:

- Did the trigger arrive?
- Was it deduplicated or filtered?
- Did routing choose the expected target?
- Did a worker actually start?
- Did the external system acknowledge?
- Did local state reconcile afterward?
- Did a retry duplicate work?
- Did a stale watcher/process retain ownership?

## 5. Test hypotheses cheaply

For each hypothesis, name one observation that would falsify it.

Prefer instrumentation or a narrow probe over speculative code changes.

Examples:

- inspect port owner before restarting a daemon,
- compare config resolution in Windows and WSL,
- record request IDs through router -> worker -> provider,
- compare intended order state with venue-reported state,
- inspect CDP target changes before blaming injection logic.

## 6. Fix the first broken invariant

Once the first invalid transition is established, fix there.

Do not add retries, sleeps, broad exception swallowing, or process restarts merely to hide the symptom unless recovery semantics explicitly require them.

## 7. Prove the repair

Repeat the original reproduction and capture before/after evidence.

Then test the nearest failure modes:

- restart/reconnect,
- duplicate trigger,
- partial external failure,
- stale state,
- cancellation/timeout,
- concurrent actor when relevant.

## Report

Return:

1. symptom and reproduction,
2. runtime topology,
3. evidence timeline,
4. ruled-out hypotheses,
5. root cause,
6. fix location,
7. before/after proof,
8. remaining uncertainty.
