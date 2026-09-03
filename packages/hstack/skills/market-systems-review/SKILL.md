---
name: market-systems-review
description: "Review market-data, trading, execution, and on-chain systems for data integrity, risk, venue semantics, order state, reconciliation, idempotency, latency assumptions, and failure containment."
---

# Market systems review

Treat a trading or market system as a chain of claims that must remain true from raw observation through reconciled outcome.

## Trace the transaction path

Map:

`source -> ingest -> normalization -> feature/signal -> decision -> risk -> order intent -> routing -> venue submission -> acknowledgement -> fill/update -> reconciliation -> PnL/record -> review`

For scanning/intelligence systems without execution, stop at the decision/output boundary but still verify source identity, freshness, normalization, and evidence.

## 1. Data integrity

For every input record, identify:

- venue/source,
- instrument identity,
- market type,
- timestamp semantics,
- freshness/staleness policy,
- units and quote/base convention,
- precision/rounding,
- whether data is live, delayed, cached, fixture, simulated, or replayed.

Do not blend spot, perpetual, futures, options, on-chain pools, tokenized equities, or synthetic products without an explicit normalization contract.

## 2. Venue adapter contract

Each venue adapter should translate external semantics into a stable internal model while retaining venue-specific facts needed for correctness.

Verify:

- symbol mapping,
- tick/lot/min notional rules,
- leverage/margin mode,
- position mode,
- reduce-only/close semantics,
- order types and time-in-force,
- client order/idempotency IDs,
- rate limits and retry rules,
- websocket reconnect/snapshot reconciliation,
- timestamp/nonce requirements.

Never assume two exchanges use the same meaning for similarly named fields.

## 3. Risk boundary

Risk should be evaluated against explicit order intent before submission.

Review:

- account and venue exposure,
- per-trade sizing,
- leverage caps,
- daily/session loss halts,
- instrument allow/deny rules,
- stale-data rejection,
- duplicate-signal protection,
- liquidation/maintenance-margin awareness,
- concurrent orders sharing the same capital.

A model or signal generator should not bypass deterministic risk constraints.

## 4. Order lifecycle

Model order state explicitly. At minimum distinguish:

- intended,
- submitted,
- acknowledged,
- open/working,
- partially filled,
- filled,
- cancelled,
- rejected,
- unknown/reconciling.

A request returning HTTP 200 does not prove the order exists in the desired state.

## 5. Idempotency and duplicates

Test:

- duplicate webhook/signal,
- process retry after timeout,
- venue acknowledgement lost locally,
- reconnect replay,
- worker restart,
- same strategy/timeframe acting twice,
- multiple accounts competing for the same assignment.

Use stable intent IDs and reconcile before resubmission when ambiguity exists.

## 6. Reconciliation

Local state must be repairable from venue/on-chain truth where possible.

Verify periodic or event-driven reconciliation for:

- open orders,
- fills,
- positions,
- balances/margin,
- funding/fees,
- realized/unrealized PnL when recorded locally.

For on-chain execution, include transaction hash, chain ID, nonce, receipt/finality assumptions, revert handling, and reorg/finality policy where relevant.

## 7. Observability

Preserve enough structured evidence to reconstruct an execution without exposing credentials:

- intent ID,
- strategy/signal identity,
- source timestamp,
- risk decision,
- account/venue/instrument,
- sanitized request metadata,
- venue order ID / transaction hash,
- fills and reconciliation state,
- error/retry timeline.

## 8. Verification

Prefer replay, sandbox, testnet, or paper environments when they can reproduce the relevant semantics safely.

If a behavior can only be proven live, state that limitation. Never silently substitute mocked data for a live-data requirement.

## Output

Return:

1. transaction/data-flow map,
2. correctness invariants,
3. risk boundary findings,
4. venue/adapter semantic findings,
5. duplicate/retry/reconciliation findings,
6. observability gaps,
7. verification evidence and environment,
8. unresolved live-risk assumptions.
