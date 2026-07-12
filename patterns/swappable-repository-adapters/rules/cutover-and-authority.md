# Cutover and authority

At every migration stage, name exactly one authoritative writer for each coherent data partition. All
acknowledged writes must first commit to that authority. The candidate may receive copies for verification,
but it does not become authoritative until the cutover gate passes and composition changes.

Record `W0`, the source position anchoring the backfill snapshot to durable capture, before an online
backfill begins. Apply backfill and replay idempotently with a source version or equivalent monotonic guard;
propagate deletions or tombstones and never allow an older snapshot value to overwrite a newer replayed
change.

Choose one migration mode deliberately:

- **Write pause:** fence mutations, record the stable source position, copy, verify, switch, and resume. In
  this mode `W0` and `Wf` may be the same position. Prefer it when the bounded interruption is cheaper than
  online synchronization complexity.
- **Durable online synchronization:** establish retained capture at `W0`, then backfill and replay the
  current authority's committed change log, change-data capture, or transactional outbox. Track offsets,
  gaps, duplicates, tombstones, poison records, retries, and reconciliation.

Do not implement online migration as a generic wrapper that calls current and candidate repositories one
after the other. A process crash, timeout, or partial rejection creates disagreement with no atomic commit.
If a datastore-specific distributed transaction truly spans both systems, document and test its failure
model rather than presenting it as portable repository behavior.

Cut over at a coherent partition boundary. Random per-request routing can send related writes and reads to
different stores. A tenant, shard, or dataset may migrate independently only when ownership and cross-
partition operations are explicit and synchronization preserves its complete history.

At cutover, fence the current authority and record `Wf`, its final accepted commit position. The candidate
must prove application exactly through `Wf`, with no unresolved gaps or poison records, before authority
changes. A low or agreed lag threshold is operational evidence only and cannot replace this correctness
gate. Fence old writers against restart after the switch.

Define rollback before cutover. To flip back immediately, the previous store must contain every write
acknowledged by the candidate, usually through a durable reverse stream whose candidate-side start position
is recorded before candidate traffic resumes. Otherwise rollback is a restore and replay operation with a
stated recovery window, or the safe response is a forward fix. Never promise a configuration-only rollback
when the data path is one-way.
