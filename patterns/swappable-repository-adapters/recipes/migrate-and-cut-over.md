# Migrate and cut over

1. Freeze the port semantics for the migration and run the shared contract plus operational acceptance
   tests against current and candidate adapters.
2. Record invariants, volume, latency and error budgets, recovery targets, authority, and rollback criteria.
   Identify incompatible or unmappable data before copying it.
3. Choose a write pause or durable online synchronization. Define how source positions are represented per
   partition and how writes are fenced. Do not introduce uncoordinated dual writes.
4. For an online migration, establish and retain durable capture before backfill and record `W0`, the source
   position anchoring the snapshot to the committed-change stream. Prove that the snapshot/log handoff has
   overlap but no gap.
5. Build an idempotent, resumable backfill with stable partitions, checkpoints, bounded batches, retry
   policy, poison-record handling, and metrics. Preserve identity and version information. Use source
   versions, positions, or an equivalent conditional apply rule so stale snapshot data cannot overwrite a
   newer replayed change.
6. Replay every relevant create, update, and deletion or tombstone committed by the current authority from
   `W0`. Track offsets, gaps, duplicates, ordering, poison records, and retries. Concurrent backfill and
   replay require a tested monotonic apply rule; otherwise complete backfill before replay.
7. Reconcile totals by partition, application invariants, missing and extra identities, and sampled
   normalized values. Investigate every unexplained mismatch.
8. Optionally shadow a bounded sample of reads. Return the current authoritative result, compare normalized
   application values asynchronously, and protect sensitive data and store capacity.
9. Rehearse cutover and rollback with production-scale data. Before cutover, establish the reverse stream or
   replayable candidate log required to preserve writes acknowledged during the rollback window.
10. Fence or drain mutations to the current authority and record `Wf`, its final accepted source position.
    Keep the fence active while the candidate catches up.
11. Require proof that the candidate applied exactly through `Wf`, with no offset gaps, unresolved poison
    records, failed invariants, or unexplained reconciliation mismatches. Do not use a nonzero lag threshold
    as the authority-transfer gate.
12. Switch composition at a coherent partition boundary, fence old writers against restart, and mark the
    candidate authoritative. Before resuming mutations, record the candidate-side start position and enable
    or confirm the declared reverse capture when immediate rollback is promised. Stop the old-to-candidate
    stream only after `Wf` is durable, then resume traffic without creating a second authority.
13. Monitor correctness, conflicts, latency, errors, saturation, replay positions, and rollback-path health
    through the stabilization window.

Abort before the authority switch if evidence is incomplete. After the switch, follow the predeclared
rollback recipe only while its data-safety preconditions remain true; otherwise prefer a forward fix or a
new controlled migration.
