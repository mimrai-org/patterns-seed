# Capability and adapter shape

Apply this pattern to one cohesive persistence capability, not automatically to the entire database.
The consumer owns a repository port under `application/persistence`. Its operations use application- or
domain-owned values and name the outcomes the consumer needs. A repository for one aggregate may load by
identity and persist a versioned state; a reporting capability may instead expose a dedicated projection
port. Do not combine both merely because they use the same database.

Each implementation lives under `persistence/adapters/<adapter>`. It owns four roles:

- A concrete repository that translates each port operation into storage calls.
- Persistence records, entities, or schemas shaped for that technology.
- A mapper between application values and those records.
- Technology-specific configuration, migrations, and integration tests.

An optional in-memory adapter is a real implementation of the same port, not a mock collection shared by
all tests. It must honor the contract's conflict, copy, ordering, and concurrency behavior or explicitly
decline to be a substitute. A test double with intentionally reduced behavior should be named as a stub or
fake and used only where its reduced contract is acceptable.

The shared contract suite belongs outside every adapter and accepts lifecycle fixtures for each one.
Bootstrap owns the mapping from configuration to one concrete repository and the lifecycle of its client
or pool. Migration programs and verification jobs belong beside persistence infrastructure; they may use
adapter internals when necessary but are never imported by application code.

In a feature- or module-oriented project, repeat this shape inside the owning capability. Do not create a
global persistence layer that becomes a second application model or lets unrelated modules query each
other's records.
