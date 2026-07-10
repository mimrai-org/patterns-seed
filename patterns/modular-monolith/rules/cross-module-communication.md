# Cross-module communication

Modules communicate through supported public operations or published integration events.

Use a synchronous public call when the caller requires the result now. Use an integration event when a
committed fact has independent reactions and the publisher does not require their results.

Do not:

- Import another module's internal file or concrete implementation.
- Reach its dependency-injection container or service locator.
- Share mutable domain objects across the boundary.
- Use an event bus to conceal a request/response dependency.
- Build a long synchronous chain or a cyclic module graph.

Contracts use stable, technology-neutral values. Expected failures are explicit. Integration events
are immutable facts with versioned schemas and documented delivery semantics.

When collaboration becomes frequent and chatty, reconsider the boundary. The modules may be one
capability, the calling module may need a local projection, or an owning workflow may be missing.
