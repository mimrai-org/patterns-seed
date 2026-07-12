# Roll back an adapter

Rollback protects acknowledged data, not merely process availability. Execute only the rehearsed path.

1. Confirm the trigger meets the declared rollback criteria and appoint one incident owner for the
   authority decision.
2. Stop, drain, or fence mutations before changing selection. Record the final candidate commit position for
   the affected partition; do not reuse the original cutover `Wf` as if no candidate-era writes occurred.
3. Determine where every write acknowledged since cutover exists. Verify the previous store has applied the
   durable reverse stream exactly through that final candidate position, with no gap or poison record, or
   stop and run the planned export/replay into it.
4. Reconcile the affected partitions and required application invariants. Do not rely only on row or
   document counts.
5. Switch bootstrap or routing so the previous adapter becomes the single authoritative writer for the
   complete partition. Fence candidate writers against restart and record the new authority checkpoint.
6. Run focused contract probes and production smoke checks, then resume mutations gradually.
7. Keep the candidate fenced from writes. Preserve its data, logs, offsets, and diagnostics for analysis
   and possible forward migration.
8. Monitor correctness, conflicts, errors, latency, and synchronization until the system is stable.
9. Record the authority transition, any lost or delayed operations, reconciliation results, and the next
   remediation decision.

If the previous store does not contain candidate-era writes and they cannot be replayed safely, a
configuration flip is data loss, not rollback. Keep the candidate authoritative, isolate the failing path,
restore from a durable source, or perform a controlled forward repair according to the incident plan.
