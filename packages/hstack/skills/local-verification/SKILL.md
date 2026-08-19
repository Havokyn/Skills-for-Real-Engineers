---
name: local-verification
description: "Create or run reproducible local verification for a project, with explicit Windows and WSL/Linux parity when relevant. Use when hosted CI is unavailable, insufficient, or not the source of truth for runtime behavior."
---

# Local verification

Verification should produce evidence another engineer can rerun from the same ref.

Do not assume GitHub Actions or another hosted CI service exists.

## Establish the verification contract

Write down:

- repository/ref and clean/dirty state,
- supported local environments,
- required toolchain versions,
- build command,
- test command,
- runtime smoke path,
- artifact(s) that must exist,
- evidence outputs to preserve.

## Layer the checks

Run the cheapest deterministic checks first:

1. repository sanity and dependency resolution,
2. format/lint/static analysis where configured,
3. type check/compile,
4. unit and integration tests,
5. artifact existence and checksums when reproducibility matters,
6. runtime smoke/e2e behavior,
7. cross-OS parity where the software claims it.

Do not add a tool just to satisfy this list. Use the repository's existing stack unless the missing verification path is itself the task.

## Windows + WSL discipline

When both environments matter:

- run native Windows verification from the Windows checkout/toolchain,
- run Linux verification through WSL2 or the project's intended Linux environment,
- record tool versions in both,
- compare outputs that should be identical,
- explicitly document platform-specific differences that are expected.

Do not claim cross-platform support from compilation alone if path handling, process spawning, shell behavior, filesystem semantics, sockets, or packaging differ.

## Runtime proof

If the change affects a daemon, API, CLI, UI, Electron host, exchange adapter, agent worker, or other running surface, include a real runtime check.

Examples of acceptable proof:

- expected health/readiness plus a real request/response,
- process starts once and survives/restarts correctly,
- CLI command produces the expected state transition,
- UI state is observed through the actual app/browser surface,
- order lifecycle is reconciled against the venue/test environment,
- agent task reaches a terminal state with tool and provenance records.

## Evidence bundle

Preserve a compact local evidence record containing:

- timestamp,
- commit SHA/ref,
- commands executed,
- exit codes,
- relevant tool versions,
- test counts,
- artifact paths and hashes when useful,
- runtime observations,
- known skips and why.

Never include secrets, auth tokens, wallet keys, cookies, or private credentials.

## Failure handling

A failed gate stops promotion of the claim it protects.

Do not relabel a failure as "flaky" without reproducing enough times or finding evidence that demonstrates nondeterminism.

## Final verdict

Use one of:

- **VERIFIED**: all required gates passed on the claimed surface.
- **PARTIALLY VERIFIED**: useful evidence passed, but named required surfaces remain untested.
- **NOT VERIFIED**: a required gate failed or the real surface could not be exercised.

Report the exact failing or missing gate rather than softening the verdict.
