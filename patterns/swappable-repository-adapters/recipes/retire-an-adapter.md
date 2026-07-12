# Retire an adapter

1. Confirm the stabilization window, correctness and operational targets, and rollback-risk acceptance are
   complete. Obtain an explicit decision that the candidate remains authoritative.
2. Run final reconciliation across all migrated partitions and resolve every unexplained mismatch.
3. Capture the backup, retention, audit, and legal disposition required for the previous data before
   destroying anything.
4. Stop shadow reads and forward or reverse synchronization in a controlled order. Verify no production
   consumer, migration worker, or recovery procedure still depends on them.
5. Remove the previous adapter from bootstrap selection, configuration schema, deployment manifests,
   dashboards, alerts, runbooks, and contract-test matrix.
6. Delete adapter code, records, mapper, client dependencies, migrations used only to operate the old
   store, and temporary comparison or backfill components.
7. Revoke old credentials, network access, and secrets. Decommission the store only after the retention and
   recovery decision allows it.
8. Run typecheck, tests, architecture checks, dependency audits, and one end-to-end path through the sole
   remaining adapter.
9. Record the final architecture decision, measured migration outcome, and any contract changes learned
   during the replacement.

Do not retain a dormant fallback indefinitely. Unexercised rollback code, stale credentials, and a slowly
diverging store create false confidence and security exposure. Keep a supported recovery mechanism, not an
undeclared second production system.
