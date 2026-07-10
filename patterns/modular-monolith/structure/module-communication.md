# Module communication

Modules collaborate through explicit public contracts. Choose the interaction from the required
semantics rather than applying one mechanism everywhere.

## Synchronous public calls

Use a public module operation when the caller needs an immediate result to complete its own outcome.
The caller depends on the target API contract imported from its root entry point and receives the
initialized API instance through module composition. It handles documented results and failures
without importing a process-global implementation. Keep call chains short and detect contract and
runtime dependency cycles in CI.

## Integration events

Use an integration event when another module reacts independently after a fact is committed. The
publishing module owns the event name and schema; subscribers do not reach into publisher internals.
Define delivery, ordering, duplication, and retry semantics. In-process delivery is acceptable when
durability is unnecessary and reactions need to occur only in the current instance. Reaching other
replicas, surviving a process failure, or guaranteeing delivery requires an explicit external channel;
an outbox and broker are separate reliability decisions.

## Orchestration

When one application outcome spans several modules, place orchestration in the module that owns the
outcome. If no existing module owns it, create an explicit coordinating capability module; the host
may wire and start that module but does not own its business decisions. Avoid a hidden global event bus
for request/response work and avoid distributed-style choreography for a simple local call.

Cycles between module APIs signal confused ownership. Merge the capabilities, move the workflow to a
clear owner, or redesign the contracts instead of masking the cycle with asynchronous delivery.
