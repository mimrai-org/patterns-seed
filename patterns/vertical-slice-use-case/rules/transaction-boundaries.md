# Consistency and transaction boundaries

A state-changing operation owns its consistency and atomicity decision. A command does not
automatically require a database transaction. One atomic statement may provide the entire guarantee,
and an operation that only invokes a remote capability may have no local transactional resource.
Document the guarantee and its failure window; do not add transaction ceremony without an anomaly it
must prevent.

When several consistency-sensitive reads or writes must succeed or fail together, start one
transaction immediately before the first such action, execute all participating persistence through
the same transaction-scoped context, and commit only after the operation's invariants hold.

Do not open and commit a separate transaction inside every repository method. That can leave a handler
partially committed while appearing successful at the unit level. Do not let the HTTP transport or a
generic controller decide business atomicity either.

Keep transactions short. Parse structural input first, and avoid waiting on remote services while
holding database locks. When an external call must influence the decision, define the failure window
explicitly; a database transaction cannot atomically include an ordinary network request.

For an effect that must follow a commit reliably, persist an outbox record in the same transaction and
deliver it with idempotent retry. For best-effort effects, execute after commit and state what loss
means. If a remote reservation or compensation protocol is required, model that workflow explicitly
rather than describing it as one local transaction.

Transactional retries require idempotency and awareness of isolation failures. Generate stable
request identities outside the retry loop, avoid duplicate non-transactional effects, and translate
exhausted conflicts to an operation-level result. Queries use transactions only when their promised
snapshot or locking semantics require one.

Do not compose independently transactional handlers and assume the outer workflow is atomic. If
several steps must commit together, give the coordinating use case one transaction-scoped capability
set or reconsider whether the split operations share one ownership boundary.
