---
name: hstack-handoff
description: "Create a compact, evidence-rich continuation record for another agent or future session. Use at phase boundaries, before pausing, after a long investigation, or when transferring work between models/agents."
---

# Handoff

A handoff is executable state, not a diary.

Write it so a fresh engineer can continue without rereading the whole conversation.

## Required sections

### Objective

One paragraph describing the destination and current phase.

### Exact repository state

Include where applicable:

- repository and local path,
- branch,
- HEAD SHA,
- worktree/clean status,
- relevant PR/issue,
- environment/toolchain.

Never invent values. Mark unavailable facts as unknown.

### What is proven

List only claims backed by evidence from the session. Include exact commands, files, tests, logs, runtime observations, or external records that support each important claim.

### What changed

Summarize files/components changed and why. Do not paste the entire diff.

### Decisions and invariants

Record architecture/risk/product decisions that the next worker must preserve.

### Verification

State:

- checks run,
- environments used,
- results,
- artifacts/evidence produced,
- skipped checks and reasons,
- final verification verdict.

### Open uncertainty

List unresolved questions, failed hypotheses, flaky behavior not yet explained, and assumptions that still need live proof.

### Next executable step

Give the single best next action, with enough detail to run it immediately. Then list at most a few follow-on steps in dependency order.

### Do not do

Carry forward explicit constraints such as no merge, no deploy, no production mutation, no hosted CI, no secrets in logs, or scope boundaries.

## Compression rule

Prefer pointers over pasted bulk.

Use exact file paths, symbols, SHAs, issue/PR references, commands, and artifact paths. Preserve raw evidence only when it is small and uniquely necessary.

## Quality gate

Before finishing, ask:

- Could a new agent identify the correct repo/ref without asking?
- Could it distinguish proven facts from guesses?
- Could it reproduce the last verification?
- Could it continue without accidentally violating a constraint?

If not, the handoff is incomplete.
