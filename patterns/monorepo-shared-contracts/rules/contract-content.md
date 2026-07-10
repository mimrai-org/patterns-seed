# Contract content

Contract packages own the externally meaningful vocabulary shared by applications. Suitable content
includes:

- Request, result, command, event, and message shapes.
- Stable discriminators, wire-level identifiers, and documented units or formats.
- Runtime schemas, parsers, and deterministic serialization metadata.
- Static types derived from the authoritative schema or generated protocol.
- Synthetic fixtures that demonstrate the supported compatibility window.

Keep these concerns out:

- Use cases, workflows, authorization decisions, and domain services.
- ORM entities, migrations, repositories, database clients, and persistence queries.
- HTTP handlers, framework request/response objects, UI components, hooks, and state stores.
- Queue, RPC, analytics, or vendor clients and application environment configuration.
- Application-specific errors, logging, retries, caching, and deployment wiring.

Structural validation belongs with the contract: required shape, encoding, discriminator, length or
range that is part of the protocol. Business policy belongs to the application: whether the current
actor may perform an action, whether state permits it, or which provider should execute it.

Do not export an application's internal model merely because its current fields resemble the wire
shape. Map between the contract value and the application-owned model at the boundary. This allows
transport, persistence, and business rules to evolve for different reasons.

Require a second real consumer before promotion from local code. When only part of a local type is
shared, extract the supported boundary surface rather than moving the whole implementation into the
package.
