# Maintain one authoritative writer

A migration names one authoritative writer per coherent data partition at every stage. Bounded write
pauses and durable replay of already committed changes are accepted synchronization strategies. An online
migration establishes retained capture and records `W0` before backfill. At cutover it fences the current
authority, records its final position `Wf`, and transfers authority only after the candidate proves exact
application through `Wf`. A nonzero lag threshold was rejected as a correctness gate because it can leave
acknowledged source commits absent or out of order at the new authority.

Uncoordinated application-level dual writes were rejected because partial success, retries, and crashes can
acknowledge divergent state without an atomic recovery record. A reversible cutover also requires a durable
reverse stream or replayable candidate log for writes accepted after `Wf`; otherwise rollback is another
data migration, not a configuration flip. This decision may add downtime, fencing, or change-capture
infrastructure, but it makes data ownership, snapshot/log continuity, cutover, and rollback safety
observable rather than implicit.
