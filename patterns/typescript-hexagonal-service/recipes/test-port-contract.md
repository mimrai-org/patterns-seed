# Test a port contract

1. List behavior every valid adapter must share: results, absence, conflicts, ordering, idempotency,
   and expected failures.
2. Build a reusable test suite that accepts an adapter factory and resource cleanup hooks.
3. Keep assertions in application vocabulary; do not expose vendor fields to the suite.
4. Run the suite against a small in-memory adapter first to prove the contract is coherent.
5. Run the same suite against each production adapter using representative resources.
6. Add adapter-specific tests only for behavior outside the common contract, such as migrations or
   retry configuration.
7. Execute the contract suite in CI whenever the port or an implementation changes.

If two adapters cannot satisfy the same observable semantics, they are not substitutes. Split the
contract or make the difference visible to the consuming use case.
