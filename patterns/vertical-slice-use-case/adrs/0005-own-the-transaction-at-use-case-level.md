# Own consistency decisions at use-case level

The state-changing use case defines its consistency guarantee and chooses among one atomic action, one
explicit local transaction, or an explicit multi-system workflow. When a transaction is required, all
participating persistence uses the same transaction-scoped context. Mandatory transactions for every
command, repository-local transactions, and transport-owned transactions were rejected because they
either add ceremony without a guarantee or can commit partial outcomes. Use-case ownership makes the
actual atomicity, retry, and external-effect semantics testable.
