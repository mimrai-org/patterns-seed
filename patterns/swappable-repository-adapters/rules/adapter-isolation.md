# Adapter isolation

Every concrete repository implements the application-owned port independently. It may import the port,
application values, its own records and mapper, and its storage client. It must not import another adapter,
bootstrap, or migration selection policy.

Bootstrap or equivalent composition code is the only steady-state owner of:

- Concrete adapter selection.
- Client, pool, credential, and shutdown lifecycle.
- Environment configuration and feature rollout at a coherent data-partition boundary.
- Construction and injection into application consumers.

Do not branch on adapter identity inside a use case. Do not retrieve a repository from a global container
or make a repository select its own backend from environment variables. These approaches hide the
dependency and turn replacement into runtime policy scattered through the application.

Avoid adapter inheritance. A shared base class often carries query assumptions, storage records, or
failure behavior from the current technology into the candidate. Extract only stateless, semantics-free
helpers after duplication is demonstrated. Prefer composition for instrumentation, but keep metrics and
tracing decorators behavior-preserving and outside data migration.

The manifest enforces application-to-persistence and persistence-to-bootstrap prohibitions for root role
paths and the canonical one-segment `src/modules/<capability>/...` layout. It cannot infer other role names,
deeper module identities, or direct imports of bare ORM and driver packages. Add project-owned restricted-
import or dependency rules for those cases; do not claim `patterns check` covers them.

The manifest's isolation rule (`within: **/persistence/adapters/*/**`) enforces sibling independence: any
import crossing from one adapter directory into another fails `patterns check`, while imports inside one
adapter remain allowed. Its identity is the captured directory name alone, so equal adapter slugs under
different owning modules or applications share an identity and their cross-owner imports are not flagged;
for that case add an import-path lint rule, package export boundary, or architecture test keyed by
`<owning persistence/adapters root, adapter directory>`.
