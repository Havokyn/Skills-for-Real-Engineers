---
name: adversarial-review
description: "Run an independent skeptical review of a diff, design, or operational claim using multiple non-overlapping lenses. Use before shipping consequential changes or when confidence is suspiciously high."
---

# Adversarial review

The goal is not consensus. The goal is to find a concrete way the work can be wrong before reality does.

## Prepare the review packet

Provide reviewers only what they need:

- task/spec or intended invariant,
- base and head refs for code changes,
- changed files or architecture artifact,
- verification evidence already produced,
- relevant constraints.

Do not prime reviewers with the implementer's confidence or preferred conclusion.

## Independent lenses

Run at least three distinct lenses for consequential work:

### Correctness lens

Look for wrong state transitions, invalid assumptions, edge cases, partial failure, ordering, stale data, and incorrect semantics.

### Architecture lens

Look for confused ownership, leaky adapters, hidden coupling, shared mutable state, lifecycle gaps, and trust-boundary violations.

### Verification lens

Try to show that the existing tests/evidence do not prove the claimed behavior. Identify the smallest additional experiment that would settle the claim.

Add domain lenses when relevant:

- agentic systems: permission, retry, cancellation, provenance, memory authority,
- market systems: venue semantics, duplicate execution, risk bypass, reconciliation,
- security: secret exposure, injection, unsafe subprocess/tool execution,
- cross-platform: Windows/WSL path, shell, process, packaging, filesystem differences.

## Evidence standard

A finding should include:

1. location or boundary,
2. violated invariant,
3. concrete failure scenario,
4. evidence or executable probe,
5. severity,
6. smallest corrective direction.

Reject findings that are only stylistic preference unless they create measurable maintenance or correctness risk.

## Resolve disagreement

When reviewers disagree, do not vote.

Run the cheapest observation that can discriminate between the competing claims. Runtime behavior, a focused test, a trace, or source-of-truth documentation outranks reviewer confidence.

## Verdict

Return one of:

- **READY**: no unresolved high-impact findings and required proof exists.
- **READY WITH FOLLOW-UPS**: only bounded non-blocking issues remain.
- **NOT READY**: a correctness, safety, verification, or architecture blocker remains.

List accepted findings, dismissed findings with reasons, and any experiment still needed.
