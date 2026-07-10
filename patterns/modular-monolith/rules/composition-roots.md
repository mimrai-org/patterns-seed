# Composition roots

Each module assembles its private implementation behind `register.ts` or an equivalent factory. It
chooses internal adapters, wires use cases, and returns an explicit surface such as
`{ api, lifecycle }`. Its factory parameters name runtime-wide facilities and public APIs from modules
it depends on; no active module instance is stored in a global singleton.

The host imports module root entry points, builds a directed acyclic module graph, creates dependencies
before dependents, and injects initialized public APIs. It may supply runtime-wide facilities such as
configuration, logging, tracing, or a database connection, but it does not construct individual module
handlers or select private repositories.

After construction, the host starts lifecycle hooks in dependency order. If construction or startup
fails, it stops already-started modules in reverse order; normal shutdown follows the same reverse
dependency order. Startup, readiness, partial failure, and cleanup are composition concerns, not hidden
side effects of importing a module.

Keep business branches out of host and registration code. Framework containers may implement the
wiring, but container lookup must not become a cross-module API.

Tests should be able to create one module with controlled infrastructure and explicit fake dependency
APIs without starting the whole host. If module construction requires private knowledge in the host,
move that knowledge behind the module registration contract.
