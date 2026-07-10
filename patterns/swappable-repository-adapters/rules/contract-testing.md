# Contract testing

Treat the behavior contract as executable compatibility evidence. Define one suite in application terms
and run it unchanged against a fixture for the current adapter, every candidate, and any in-memory adapter
that claims substitutability.

Each fixture must provide:

- A repository through the application-owned port.
- Isolated, representative backing state.
- Deterministic reset or unique namespace behavior between tests.
- Resource startup, readiness, and cleanup hooks.
- Optional capability metadata only when the port itself declares optional capabilities; never adapter-
  name conditionals.

The suite covers every promised result and failure, mapper-visible round trips, conflicts, and consistency
guarantees. Assertions must not import storage records or inspect driver calls. Run concurrent operations
when the contract promises optimistic locking or uniqueness; a sequential imitation is insufficient.

An in-memory adapter is useful for fast feedback but cannot certify a database adapter. Run production
adapters against a real database or an operationally representative disposable resource in CI. Do not
substitute a different storage engine merely because a client API is compatible.

Keep adapter-specific tests separate. Schema migration, index selection, client retry configuration,
timeouts, and vendor diagnostics are important but not common behavior. Conversely, never move a failed
common example into an adapter-specific suite to make the candidate green.

Version the contract with the port. A changed absence result, ordering rule, conflict policy, or atomicity
promise is an application contract change and must be reviewed before every adapter and fixture is updated.
