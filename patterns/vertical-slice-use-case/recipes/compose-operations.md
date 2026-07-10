# Compose operations

1. State the final outcome and identify its owner. Distinguish independent presentation aggregation,
   one business workflow, and an independent reaction to a committed fact.
2. For presentation-only aggregation, create a composite query operation or a purpose-built read
   model. Avoid serial calls that produce avoidable latency.
3. For a business workflow, create a coordinating operation with its own input, result, idempotency,
   and failure contract.
4. Define coordinator-owned ports for the smallest capabilities required from each step. Do not
   import sibling handlers, adapters, or containers.
5. Inject an existing callable directly only when it already satisfies a port. Otherwise, implement a
   workflow-owned adapter at an explicit outer composition edge that translates contracts and
   failures without joining either operation's internals. Place it under a deliberate composition or
   enclosing-module integration root. Bootstrap constructs and injects the callable or adapter; it
   contains no mapping or workflow policy.
6. Assign ordering, timeout, retry, compensation, and partial-failure decisions to the coordinator.
   Reject a cyclic operation graph.
7. If all steps require one local transaction, construct transaction-scoped capabilities under the
   coordinator or merge the state changes behind one use-case owner. Do not nest handlers and assume
   atomicity.
8. For an independent reaction, publish only after the initiating outcome is durably accepted—after
   commit for locally persisted state—and document delivery, ordering, deduplication, and replay
   semantics.
9. Test the coordinator through its public outcome, then add composition tests for wiring and failure
   paths. Keep step-specific tests with their original operations.
10. Run sibling-import and cycle checks with test files included.

If coordination appears in many callers or requires access to private state from every participant,
revisit the boundaries instead of adding another facade over the coupling.
