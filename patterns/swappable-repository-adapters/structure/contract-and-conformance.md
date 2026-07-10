# Contract and conformance

A TypeScript interface proves that an object has assignable members at compile time. It does not prove
what those members do, and the type itself is absent at runtime. Substitutability therefore requires an
executable behavior contract.

Write the contract in application vocabulary. For every operation the port exposes, decide which of these
dimensions are observable and required:

- Identity, value round trips, defaulting, precision, and treatment of historical record versions.
- Missing-data results and whether deletion of an absent value succeeds or conflicts.
- Duplicate creation, uniqueness, stale-version, and other conflict outcomes.
- Ordering, page boundaries, cursor stability, filtering, and visibility of recent writes.
- Atomicity, isolation, optimistic concurrency, idempotency, and retry behavior.
- Translation of expected storage failures into application-owned outcomes.

The shared suite is a test function or package that receives an adapter fixture. The fixture creates an
isolated representative resource, returns a repository through the application port, resets state between
examples, and releases clients afterward. Assertions depend only on application values and outcomes. Run
the same examples unchanged against each claimed substitute.

Raw records from supported historical schemas remain adapter-specific fixtures. When reading historical
state is a common application guarantee, the shared fixture may expose a semantic setup hook such as
“seed supported historical state”; each adapter implements that hook with its own record shape while the
unchanged shared assertion observes only the application value or outcome.

The suite is the compatibility floor, not the entire adapter test set. Technology-specific integration
tests still cover migrations, indexes, record constraints, driver configuration, timeouts, and failure
injection. Performance, capacity, availability, and operational recovery need separate acceptance gates;
functional equivalence does not make two stores operationally equivalent.

When one adapter cannot pass a required example, do not add an adapter-name conditional to the suite. The
options are to fix the implementation, narrow the common guarantee, split the port, or make a changed
application policy explicit. A conditional contract documents non-substitution while pretending to prove
the opposite.
