# Port design

Design a port from the consuming use case outward.

- Name the capability in application vocabulary, not after a vendor or protocol.
- Include only operations required by a cohesive group of consumers.
- Accept and return domain or application-owned values.
- Make absence, conflicts, retries, and expected failures explicit in the contract.
- Define ordering, idempotency, consistency, and time semantics when correctness depends on them.

Avoid universal repository or provider interfaces with every operation a tool supports. They violate
interface segregation, make adapters harder to substitute, and let infrastructure shape application
policy.

Place a port near its only consumer. Promote it to a shared `application/ports` area only when several
use cases need the same semantics. A TypeScript type match is not enough: use contract tests when more
than one adapter claims equivalent behavior.
