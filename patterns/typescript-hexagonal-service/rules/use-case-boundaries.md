# Use-case boundaries

A use case represents one application outcome. It coordinates domain behavior and external
capabilities without implementing protocol or infrastructure details.

A use case should:

- Accept a technology-neutral input.
- Establish application-level authorization and preconditions when relevant.
- Load required state through ports.
- Delegate invariants and state transitions to domain objects.
- Persist or publish through ports with explicit consistency semantics.
- Return a technology-neutral result or expected failure.

Do not turn a use case into a pass-through wrapper around one adapter. Do not put business rules in an
inbound controller or persistence mapper. When several use cases share a true domain rule, move the
rule into a domain object or service; when they share orchestration, first confirm they are not one
cohesive use case.

Keep transaction scope visible. Separately injected ports do not share a transaction automatically. If
several writes must commit atomically, choose one explicit contract:

- A single outbound port owns the complete atomic persistence operation; or
- A transaction runner invokes the use case with transaction-scoped ports bound to the same resource.

Test both commit and rollback. When effects cannot share one transaction, model compensation,
idempotency, or eventual consistency instead of claiming atomicity.
