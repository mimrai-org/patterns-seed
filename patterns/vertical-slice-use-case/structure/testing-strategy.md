# Testing strategy

Tests follow the operation and verify behavior at the narrowest boundary that can expose the risk.

- Contract and validation tests cover accepted representations, rejected shapes, normalization, and
  stable expected-failure values without starting a server.
- Handler behavior tests invoke the operation with controlled ports and assert outcomes and important
  interactions. Prefer state and result assertions over the order of private method calls.
- Handler integration tests use real persistence when queries, constraints, isolation, transactions,
  or mapping are part of correctness. A mock cannot prove database semantics.
- Port contract tests are shared by multiple implementations only when the same operation-owned
  contract genuinely has more than one adapter.
- Adapter integration tests cover provider mapping, timeouts, retries, error translation, and resource
  cleanup against the closest practical external system.
- Transport tests verify protocol parsing, authentication context mapping, status or acknowledgement
  semantics, and expected failure translation.
- Architecture tests reject cross-slice imports, handler-to-edge dependencies, cycles, and forbidden
  aliases or re-exports.
- A small end-to-end suite proves representative wiring, declared consistency behavior, and
  process-level error handling without duplicating every handler case.

Colocate tests under the operation or next to the file they verify. Test code may access its own
slice's private helpers but must obey sibling isolation and must not use another slice's fixtures as a
back door. Put domain-neutral test harnesses in shared test support only after several independent
consumers prove the abstraction.

For state-changing operations, test the declared consistency guarantee, concurrent execution,
duplicate delivery or retry, and the failure window around external effects. When the contract
promises a transaction, include rollback after a mid-operation failure and the boundary between
database commit and external effects. When it does not, prove the atomic statement or explicit
workflow semantics actually claimed. For queries, test ordering, pagination, absence, and the
consistency level promised by the contract.

Run architecture checks with tests included. A production graph that passes while fixtures create
cross-slice dependencies still teaches contributors the wrong structure.
