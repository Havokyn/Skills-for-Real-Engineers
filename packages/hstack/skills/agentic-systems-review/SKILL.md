---
name: agentic-systems-review
description: "Review an agentic system for orchestration, tool boundaries, model routing, memory/provenance, permissions, retries, cancellation, idempotency, observability, and verification. Use for agent runtimes, MCP servers, multi-agent workflows, autonomous builders, and AI operating systems."
---

# Agentic systems review

Review the system as a distributed execution platform, not as a collection of prompts.

## Build the execution graph

Trace:

`request -> planner/controller -> delegation -> worker/model -> tool call -> external effect -> result -> memory/evidence -> verification -> terminal state`

Name every hop that crosses a process, model, tool, network, queue, filesystem, database, or trust boundary.

## Review dimensions

### 1. Orchestration

- Who owns the task state machine?
- Can two controllers believe they own the same task?
- Are child tasks addressable and cancellable?
- Are terminal states explicit?
- What happens when the controller restarts?

### 2. Tool contracts

- Are tool schemas narrow and typed?
- Is untrusted model output validated before mutation?
- Are read and write capabilities distinguishable?
- Can a tool be retried safely?
- Are destructive or irreversible operations gated?

### 3. Model routing

- Is routing based on task shape rather than arbitrary preference?
- Are model-specific assumptions isolated from domain logic?
- Can the system record which model/version produced a decision or artifact?
- Does failure fall back safely without silently changing semantics?

### 4. Memory and provenance

Separate:

- source-of-truth data,
- observations/evidence,
- derived summaries,
- model hypotheses,
- durable decisions,
- ephemeral conversation context.

A model-generated summary must never silently replace authoritative source data.

For durable memory writes, record source, time, task/run identity, and enough provenance to audit why the fact exists.

### 5. Concurrency and isolation

- Do parallel agents write the same branch, file, task record, account, or external object?
- Can ownership be partitioned instead of locked?
- Are worktrees/sandboxes/directories isolated?
- Are shared resources keyed idempotently?

### 6. Retry, cancellation, and recovery

Test at least conceptually:

- worker dies after side effect but before acknowledgement,
- controller retries a completed tool call,
- cancellation arrives during an external request,
- provider times out but later succeeds,
- memory write succeeds while task status write fails,
- daemon/controller restarts with in-flight work.

### 7. Observability

A run should be explainable without reading the model's private reasoning.

Prefer structured records for:

- task/run IDs,
- parent/child relationships,
- tool calls and results,
- timestamps/durations,
- model/provider selection,
- state transitions,
- verification verdicts,
- errors/retries/cancellations.

Do not log secrets or unnecessary user data.

### 8. Verification

Prove complete tasks, not merely successful model responses.

For a representative workflow, verify:

1. request accepted,
2. correct worker/tool path selected,
3. expected external effect occurred,
4. result was reconciled,
5. evidence/provenance persisted,
6. terminal state is correct,
7. restart or retry does not duplicate the effect.

## Output

Report findings by severity:

- **Critical**: can cause unauthorized/destructive effects, unrecoverable state corruption, duplicate financial/external actions, or loss of control.
- **High**: breaks task correctness, provenance, recovery, or isolation under realistic failures.
- **Medium**: creates operational ambiguity, weak verification, or avoidable coupling.
- **Low**: maintainability or clarity issue with limited runtime impact.

For each finding include the violated invariant, evidence, likely failure scenario, and smallest corrective direction.
