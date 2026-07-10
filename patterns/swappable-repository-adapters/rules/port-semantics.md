# Port semantics

Define the repository from the consuming capability outward. Its name and operations should describe
application intent, not a storage mechanism. Prefer a capability-specific contract such as loading one
aggregate for change and saving it with an expected version over `Repository<T>` with create, read,
update, delete, arbitrary filters, includes, and raw transactions.

For every operation, document:

- Application-owned input and result values.
- The meaning of absence, duplicate identity, stale version, and deletion.
- Expected failures the consumer can act on, distinct from unexpected infrastructure failure.
- Ordering, pagination, consistency, idempotency, and atomicity when observable.
- Whether cancellation, deadlines, or retry tokens are part of application policy.

Use discriminated application outcomes or deliberately mapped errors for expected cases. Do not return a
vendor exception and ask use cases to inspect its code. Keep diagnostics available through error causes,
structured logs, and telemetry without making application branches depend on a driver.

Expose only queries that a cohesive group of consumers requires. If a reporting screen needs flexible
projections while a command use case needs aggregate persistence, use separate ports. Do not grow a
generic query specification language unless query composition is itself an application requirement and
every adapter can honor its semantics.

A port is justified by replacement pressure, important isolated tests, or protection from persistence
volatility. One interface plus one implementation is acceptable when it protects such a boundary; one
interface per trivial data function is ceremony. Revisit the seam if its operations merely mirror the
current client.
