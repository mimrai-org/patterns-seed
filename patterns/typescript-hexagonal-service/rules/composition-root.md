# Composition root

Concrete dependency selection happens only under `src/bootstrap`.

Bootstrap may:

- Parse and validate configuration.
- Create clients, pools, queues, clocks, and other resources.
- Construct outbound adapters and inject them into use cases.
- Attach use cases to inbound adapters.
- Start the runtime and release resources on shutdown.

Domain, application, and adapter code must not read global containers to locate dependencies. A
framework dependency-injection container is an implementation detail of bootstrap; keep its tokens,
modules, and decorators at the edge when possible.

Separate construction from execution. Factory functions may assemble coherent groups, but business
branches do not belong in wiring code. A test can compose the same use cases with in-memory adapters
without starting the production runtime.
