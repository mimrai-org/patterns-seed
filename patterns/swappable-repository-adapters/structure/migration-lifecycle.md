# Migration lifecycle

A repository seam changes code dependencies; it does not move data by itself. Treat replacement as a
state machine with explicit evidence and data authority.

```text
baseline -> compatible candidate -> capture W0 -> backfill + committed-change replay
         -> fence current writer at Wf -> exact catch-up -> authority transfer
         -> stabilization -> previous adapter retired
```

## Baseline

The current adapter is authoritative. Freeze and run the behavior contract against it, record production
invariants and operational targets, and identify data that cannot be represented by the candidate.

## Compatible candidate

The candidate passes the shared contract and its own integration, migration, capacity, and recovery
checks. Application policy still uses the current adapter. A schema incompatibility is resolved before
data movement, not hidden in a permissive mapper.

## Synchronized candidate

The migration records two source positions for each coherent partition:

- `W0` anchors the data snapshot to durable change capture. Establish the capture mechanism and retain its
  log from `W0` before starting an online backfill. A snapshot facility may return `W0` atomically; otherwise
  use a documented overlap that cannot omit commits.
- `Wf` is the final commit position accepted by the current authority after writes are fenced at cutover.
  The candidate must apply every relevant committed change through `Wf` before it becomes authoritative.

Backfill is resumable, idempotent, checkpointed, and derived from the snapshot associated with `W0`.
Candidate writes carry a source version or position, or use an equivalent conditional rule, so an older
snapshot row cannot overwrite a newer replayed change. Replay includes creations, updates, and deletions or
tombstones; it preserves source order where the contract requires it and exposes gaps, duplicates, poison
records, retries, and offsets. Replaying after backfill is simplest. Concurrent replay is valid only when
the candidate's apply rule is monotonic and tested.

For a bounded migration, fence writes before the final copy; `W0` and `Wf` may be the same stable source
position. For an online migration, keep the current store authoritative and replay only changes already
committed to it, using database change capture, a transactionally recorded outbox, or an equivalent durable
log. Reconcile totals by partition, missing and extra identities, application invariants, and normalized
values at the required coverage.

Shadow reads may compare normalized results while returning only the authoritative response. They must be
sampled, bounded, privacy-safe, and observable. They are evidence, not a silent fallback.

## Cutover and stabilization

Fence or drain writes to the current authority and record `Wf`. Wait until replay proves that the candidate
has applied exactly through `Wf`; a time-based or nonzero lag threshold is insufficient. Fail the gate when
there is an offset gap, unresolved poison record, failed invariant, or unexplained reconciliation mismatch.
Only after this gate passes may composition change so the candidate becomes the single authoritative
writer. Fence old writers against restart, record the authority transition, and stop the old-to-candidate
stream only after its application through `Wf` is durable.

If immediate rollback is required, establish and rehearse its data path before cutover. Once the candidate
is authoritative, a durable reverse stream may copy candidate commits to the previous store. Record its
candidate-side starting position before the first candidate-authority write; its direction must be explicit
and it must not re-enable the previous writer. Without that path, preserve a replayable candidate log from
the same position and state the export/replay recovery window.

An immediate rollback is safe only if writes acknowledged after cutover also reach the previous store
through a durable reverse path, or can be replayed without loss. Otherwise recovery requires another data
movement or a forward fix. Keep the old adapter and data until that risk is accepted and the stabilization
window closes; only then retire synchronization, code, credentials, and storage.
