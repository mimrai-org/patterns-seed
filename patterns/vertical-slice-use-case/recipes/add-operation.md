# Add an operation

1. Name one observable outcome, its caller, owner, and reason to change. Decide whether it is a query,
   a state-changing command, a scheduled job, or a message reaction.
2. Define technology-neutral input, success, and expected failure values in
   `src/operations/<operation>/contract.ts`. Include actor, idempotency, or correlation values only
   when the operation uses them.
3. Add structural parsing in `validation.ts`; bound input size and keep current-state decisions out of
   the schema.
4. Implement `handler.ts` with the simplest policy model that protects the outcome. Define narrow
   slice-owned ports only for external capabilities the handler actually consumes.
5. Decide the consistency boundary. For a state change, state whether one atomic action is sufficient,
   one explicit transaction is required, or a multi-system workflow owns the outcome. Make retry,
   concurrency, and external-effect semantics explicit before adding repositories or provider calls.
6. Implement private adapters or bind existing process facilities to the slice-owned ports. Translate
   external values and failures at that boundary.
7. Add `transport.ts` to map one protocol into the contract, invoke the typed handler, and map expected
   results back. Do not put application branching in the transport.
8. Wire the adapter, handler, and transport in bootstrap. Use direct invocation unless an existing
   dispatcher is an intentional application convention.
9. Add validation, handler behavior, relevant adapter integration, and transport tests. Add transaction
   tests only when the operation promises transactional atomicity. Include an architecture fixture
   proving the slice does not import a sibling.
10. Run typecheck, tests, cycle detection, resolver-aware architecture checks, and
    `patterns check --include-tests`.

Do not edit a central service or universal repository merely to register the operation. Bootstrap may
add wiring; policy and contracts remain owned by the new slice.
