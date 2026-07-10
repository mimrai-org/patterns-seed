# Cross-slice composition

Composition needs an owner; it is not a reason for arbitrary sibling imports. First classify the
collaboration.

## Independent presentation

When an edge response merely combines independent query results, create a composite query operation or
let an outer presentation owner invoke explicit query capabilities. Keep business decisions out of
bootstrap and avoid a long serial call chain when a purpose-built read model would be clearer or
faster.

## One business workflow

Create a coordinating operation when the sequence, compensation, or final outcome is itself business
behavior. The coordinator declares narrow ports for the capabilities it needs. When an existing
handler or public module API already satisfies a port exactly, bootstrap may inject that callable
directly. Otherwise, a workflow-owned adapter at an explicit outer composition edge translates between
the coordinator port and the supported callable surface. It lives outside both operation internals;
give it a deliberate location such as `src/composition/<workflow>/` or the enclosing module's
integration area. Bootstrap only constructs and injects it; it never owns mapping, branching, retry,
or compensation policy. The coordinator does not import sibling implementations or discover them
through a container.

The coordinator owns ordering, timeout, partial failure, idempotency, and the result exposed to its
caller. A step remains owned by its original slice. If the coordinator and its steps must share mutable
domain objects or one transaction on nearly every change, the slices are probably fragments of one
larger operation or cohesive capability and should be merged or placed behind a module boundary.

## Independent reaction

Use an event after the initiating outcome is durably accepted when the operation does not require the
reaction's result. For locally persisted state, that means after its successful commit. Define delivery
semantics explicitly: in-process best effort, durable at-least-once, ordering, retry, and deduplication
are different contracts. A message bus does not make a synchronous workflow decoupled when the
publisher still depends on the consumer's success.

Operation slicing often lives inside a larger capability or modular architecture. Cross-capability
contracts belong to that enclosing boundary, not in a domain-neutral `shared` directory. If no outer
module exists, put translation in an explicit workflow-owned composition adapter or introduce a
deliberately owned contract package rather than exposing private slice files. The manifest's canonical
role globs still match an `operations/<operation>` root nested inside a module; public module APIs,
registration, and data ownership remain responsibilities of the enclosing architecture and its checks.

Reject cyclic coordination. A cycle indicates misplaced ownership, an implicit workflow, or a data
model that several slices are trying to own.
