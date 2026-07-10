# Testing layers

Use the narrowest test that proves each claim, but do not replace storage behavior with mocks when that
behavior is the claim.

## Application tests

Exercise use cases through a small repository fake or in-memory adapter. Cover application policy without
starting the production store. Do not assert ORM calls or mapper internals. If the test double weakens a
guarantee, make that limitation visible in its name and keep the guarantee covered elsewhere.

## Shared contract tests

Run identical observable examples against every claimed substitute. Use fresh state, deterministic
fixtures, unique identities, and explicit setup/reset/close hooks. Include real backing resources for
semantics that an in-process substitute cannot reproduce, especially uniqueness, transactions,
concurrency, ordering, and serialization.

## Adapter integration tests

Cover mapper round trips, old record versions, queries, indexes, driver errors, timeouts, retries, and
resource lifecycle. These tests may inspect persistence details and differ by technology. Keep them beside
the adapter. Raw serialized historical records belong here, not in the shared contract package. A common
historical-read promise may use an adapter-owned semantic setup hook, but shared assertions still inspect
only application values and outcomes.

## Migration tests

Run backfill twice to prove idempotency, interrupt and resume from checkpoints, replay duplicates and
out-of-order changes according to the synchronization contract, and reconcile deliberately corrupted data.
Open capture at `W0`, then create, update, and delete data while backfill runs; prove no commit is skipped and
stale snapshot data cannot overwrite newer replay. Fence at `Wf`, inject offset gaps and poison records, and
prove authority cannot switch until application through `Wf` is exact. Test rollback with writes accepted
after cutover; a configuration flip alone is not a data-safety test.

## End-to-end and operational tests

Verify one production-like path through bootstrap and the selected adapter. Separately test throughput,
latency, capacity, backup restore, credential rotation, monitoring, and failure recovery. Passing the
functional contract establishes behavior compatibility, not production readiness.

Architecture checks should scan source and tests. Contract fixtures may import adapters, but production
application tests must not create a hidden dependency on a concrete implementation. Exercise allowed and
forbidden edges for both root and canonical module layouts, including equal adapter slugs under different
owning roots, and separately verify restricted external package imports that manifest boundaries cannot
observe.
