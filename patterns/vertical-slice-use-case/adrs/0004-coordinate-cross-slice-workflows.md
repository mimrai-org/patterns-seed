# Coordinate cross-slice workflows explicitly

A business workflow spanning operations receives a coordinating operation that consumes injected
capabilities, while sibling implementation imports are prohibited. Contract translation lives in an
adapter owned by the workflow at an explicit outer composition edge, and bootstrap only constructs and
wires it. Ad hoc handler chaining and bootstrap-owned mapping were rejected because they hide
ownership, cycles, and partial failure; an explicit coordinator owns ordering, compensation,
idempotency, and the final result.
