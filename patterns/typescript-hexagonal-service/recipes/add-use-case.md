# Add a use case

1. Name one application outcome and define its technology-neutral input, result, and expected failures.
2. Identify business invariants and place them in domain objects or a domain service.
3. List the external capabilities the orchestration needs.
4. Reuse a port only when its semantics already match; otherwise define a narrow application-owned
   port near the use case.
5. Decide the consistency boundary. For atomic writes, use one atomic port or a transaction runner that
   supplies transaction-scoped ports; never assume independent adapters share a transaction.
6. Implement the orchestration under `src/application/use-cases/<use-case>/`.
7. Write a use-case test with in-memory or recording port implementations and test commit/rollback when
   atomicity is part of the contract.
8. Wire the use case in bootstrap; do not instantiate production adapters inside it.
9. Expose it through an inbound adapter only after the application contract is stable.
10. Run typecheck, tests, architecture checks, and `patterns check --include-tests`.

If the implementation is only transport validation plus one external call and contains no durable
policy, consider a simpler layered handler instead of adding hexagonal ceremony.
