# Consistency and transactions

State only guarantees the application requires and every substitute can provide. Persistence semantics
that affect application decisions belong in the port contract even though their mechanisms stay inside
adapters.

Decide explicitly:

- Whether a successful save is immediately visible to subsequent reads through the same port.
- Whether create and update detect duplicate identity or stale versions atomically.
- Which operations are atomic together and at what isolation level.
- Whether list results have deterministic order and stable pagination under concurrent writes.
- Whether retries are safe, and which idempotency or expected-version value makes them safe.
- How transient unavailability differs from application conflicts.

Prefer a capability operation that performs one required atomic state change over exposing a raw
transaction object. A driver transaction, entity manager, or session in the port leaks technology and is
rarely substitutable. If a use case genuinely needs several repository operations to share a transaction,
define an application-owned transaction runner that supplies transaction-scoped capabilities and require
every adapter to prove commit and rollback behavior.

Do not weaken production semantics to match an easy in-memory implementation. Make the in-memory adapter
perform expected-version checks and clone values when aliasing would otherwise violate persisted-value
semantics. If a candidate cannot satisfy required atomicity or visibility, split responsibilities or
change application policy explicitly; retries and eventual consistency are not transparent substitutes
for a transaction.

Contract tests establish functional guarantees. Load, failure, and recovery tests must separately confirm
that those guarantees remain credible under expected production concurrency and outages.
