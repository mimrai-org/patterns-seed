# Build the contract suite

1. Inventory each port operation and write a behavior table for success, absence, duplicates, stale
   versions, deletion, ordering, pagination, consistency, idempotency, and expected failures.
2. Remove cases the application does not require rather than requiring the least-common-denominator API of
   every storage technology.
3. Define a fixture contract that creates a repository through the application port, isolates state, resets
   or namespaces each example, and closes external resources.
4. Write assertions only against application-owned values and outcomes. Do not inspect SQL, documents,
   ORM calls, or adapter classes.
5. Include mapper-sensitive application values. Keep raw historical records in each adapter's integration
   suite. If historical-read compatibility is a common promise, give the shared fixture a semantic setup
   hook that adapters implement privately, and assert only the resulting application value or outcome.
   Test concurrent behavior with actual concurrency when uniqueness or optimistic versioning is promised.
6. Run the suite against the current production adapter first. A failure may reveal undocumented current
   behavior that must be accepted, changed explicitly, or excluded from the port.
7. Run it against each candidate and any in-memory adapter that claims to substitute. Keep the test bodies
   identical; only fixtures differ.
8. Add adapter-specific integration suites for mechanisms outside the common contract.
9. Run the contract matrix in CI whenever the port, application values, mapper, schema, client, or adapter
   changes.
10. Treat a changed contract example as a versioned compatibility decision and update every adapter before
    merging it.

Do not parameterize expected outcomes by adapter name. Optional behavior is valid only when it is an
explicit capability in the port and consumers handle its absence; otherwise a conditional suite conceals
non-substitution.
