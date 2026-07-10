# Prove substitution with a shared contract

Every claimed repository substitute runs the same executable behavior suite through an adapter fixture.
TypeScript structural assignability and mocks of driver calls were rejected as sole evidence because they
cannot establish runtime results, conflicts, ordering, atomicity, or mapping fidelity. The shared suite
adds external-resource setup and maintenance, and operational equivalence still needs separate tests, but
it makes the application's functional compatibility claims explicit and repeatable.
