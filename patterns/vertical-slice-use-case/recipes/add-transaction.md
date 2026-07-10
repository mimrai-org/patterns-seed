# Add a transaction when atomicity requires one

1. List the reads and writes that must be observed atomically and the anomaly the transaction prevents.
   If no local transactional resource participates, or one atomic statement already provides the
   guarantee, document the actual semantics and stop.
2. Choose the isolation, conflict, timeout, and retry semantics required by the operation rather than a
   global maximum-isolation default.
3. Define a slice-owned transaction capability or composition function that creates all required
   persistence ports from one transaction context. Keep the raw driver handle out of the handler.
4. Begin immediately before the first consistency-sensitive read or write. Parse structural input and
   perform unrelated work before opening the transaction.
5. Execute every atomic write through the transaction-scoped ports and commit only after invariants
   hold. Translate expected constraint or conflict failures to operation results.
6. Move ordinary remote calls outside the transaction. If an external effect must happen reliably
   after commit, persist an outbox item atomically and deliver it idempotently.
7. Generate stable operation and message identities outside retry loops. Ensure a retry cannot repeat
   a non-transactional side effect.
8. Test successful commit, failure rollback, concurrent execution, retry exhaustion, duplicate
   delivery, and the commit-to-effect boundary against real persistence.
9. Instrument transaction duration, conflict, retry, and outbox lag without logging sensitive payloads.
10. Verify no repository starts an independent transaction and no composed handler hides a nested
    commit.

Do not claim atomic behavior across a database and an ordinary provider API. Use an explicit workflow,
reservation, compensation, or durable message protocol when the outcome spans systems.
