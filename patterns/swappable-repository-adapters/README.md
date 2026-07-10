# Swappable repository adapters

Isolate one meaningful persistence capability behind a narrow contract owned by the code that consumes
it. Each storage technology implements that contract in its own adapter, owns its persistence records
and mappers, and proves the same observable semantics through a shared contract suite. Bootstrap selects
one adapter; application policy never imports the selection or the storage tool.

```text
compile time:  bootstrap -> selected adapter -> application-owned repository port
runtime:       use case  -> repository port  -> injected adapter -> backing store

replacement:  current adapter ── contract ── candidate adapter
              capture W0 -> backfill + replay -> fence Wf -> authority transfer
```

This is a local persistence pattern, not a complete hexagonal architecture. It does not prescribe how
HTTP, messaging, domain modeling, or every other external effect is organized. Apply it to a capability
whose persistence technology needs isolation; a project may remain layered, feature-oriented, modular,
or hexagonal around that seam.

## Use it when

- A persistence technology is likely to change, already has two implementations, or materially slows
  isolated application tests.
- Storage records or query APIs are leaking into application policy and create costly coupling.
- A migration must preserve documented behavior across old and candidate stores.
- The team can state the repository's absence, conflict, ordering, concurrency, consistency, and failure
  semantics precisely enough to test them.

## Avoid it when

- The application is simple CRUD over one stable store and replacement is speculative.
- The proposed interface only renames every method of an ORM or database client.
- Two stores cannot offer the same required semantics; they need different ports or an explicit policy
  change, not a claim of transparent substitution.
- A read-only query module or direct data-access function would provide the needed seam with less
  ceremony.

## Reference shape

```text
src/
├── application/
│   ├── model/                         application- or domain-owned values
│   ├── persistence/
│   │   └── <capability>-repository.ts narrow port and documented outcomes
│   └── use-cases/                     consumers of the port
├── persistence/
│   ├── adapters/
│   │   ├── memory/                    optional fast implementation with honest semantics
│   │   ├── current/
│   │   │   ├── records/               store-specific records or entities
│   │   │   ├── <capability>-mapper.ts
│   │   │   ├── <capability>-repository.ts
│   │   │   └── migrations/
│   │   └── candidate/                 same roles for a prospective replacement
│   └── migration/
│       ├── backfill/                   resumable data movement
│       └── verification/               reconciliation and comparison
└── bootstrap/
    └── persistence.ts                 adapter selection and resource lifecycle

test/
├── persistence-contract/              one suite, many adapter fixtures
├── persistence-integration/           technology-specific behavior
└── persistence-migration/             backfill, catch-up, cutover, and rollback
```

Names are roles, not mandatory directory names. In a modular codebase, keep the port and adapters inside
the owning module and preserve the same dependency direction. Promote shared persistence utilities only
when they contain no capability policy and no adapter imports.

The manifest enforces both the root shape above and this canonical one-segment module shape, including
hybrid imports between them:

```text
src/modules/<capability>/application/   port and consumers
src/modules/<capability>/persistence/   concrete adapters and migration code
src/modules/<capability>/bootstrap/     optional module composition
src/bootstrap/                          optional executable-wide composition
```

`patterns check` matches resolved local imports in those exact role paths. It cannot infer arbitrary
directory names, nested module identities, or direct imports of bare ORM and driver packages. For another
layout, add a project-owned architecture rule aligned with its paths. For external packages, apply an
import-path restriction or dependency architecture test to application files. These checks complement the
manifest; the manifest must not be described as enforcing imports it cannot observe.

## Non-negotiable decisions

1. Shape the port around one cohesive application capability. Do not create `Repository<T>` with
   universal CRUD, arbitrary filters, or a transaction escape hatch.
2. Keep driver, ORM, schema, record, cursor, and vendor-error types inside an adapter.
3. State observable semantics before implementing a second adapter. Matching method signatures is not
   evidence of matching behavior.
4. Run the same contract suite against every claimed substitute, using the real backing technology where
   its behavior matters.
5. Select the concrete adapter only in bootstrap or equivalent composition code.
6. During an online migration, establish durable capture and record source position `W0` before the
   backfill snapshot. Replay every committed create, update, and deletion from that point without allowing
   stale backfill data to overwrite a newer source version.
7. Fence or drain the current writer, record its final accepted position `Wf`, and transfer authority only
   after the candidate has applied exactly through `Wf` with no gaps or poison records. A nonzero lag
   threshold is not a correctness gate.
8. Promise rollback only when acknowledged candidate writes can be preserved in the previous store or
   replayed into it through a rehearsed durable path.

## Costs and failure modes

- The seam adds a port, mapper, fixtures, integration infrastructure, and composition. Spend that cost at
  a real volatility or test boundary.
- An in-memory adapter often has stronger immediacy and simpler concurrency than a remote store. If it
  passes while production fails, the shared contract is incomplete or the test double is dishonest.
- A generic repository exposes the least common denominator, grows into a query language, and moves
  storage policy into application code.
- A mapper can silently lose precision, defaults, identifiers, or version information. Round-trip and
  historical-record tests must cover intentional and accidental loss.
- Random per-request adapter flags can split related data across stores. Select one authority per coherent
  data partition, not per call.
- Read fallback to the old store hides migration holes and makes retirement impossible. Use sampled shadow
  comparison for evidence; keep fallback an explicit incident action if it is safe at all.
- Uncoordinated dual writes can acknowledge a change after only one store commits. Prefer a write pause for
  a bounded migration or a durable committed-change stream for an online migration.
- Starting capture after backfill creates a write gap; applying a snapshot after replay can resurrect stale
  values; omitting tombstones can resurrect deleted data. Anchor the snapshot and log at `W0`, and make
  candidate application idempotent and monotonic by source version or position.
- “Low lag” is operational evidence, not proof of completeness. The final cutover gate is application
  through the fenced source position `Wf` plus successful reconciliation.

Start with [the capability shape](structure/capability-and-adapter-shape.md), then use
[the contract-suite recipe](recipes/build-the-contract-suite.md). For an existing direct dependency,
follow [introduce a persistence port](recipes/introduce-a-persistence-port.md). Agents should begin with
`patterns.yaml` and use [AGENTS.md](AGENTS.md) as the router. Research provenance is recorded in
[SOURCES.md](SOURCES.md).

Original content is licensed under [CC BY 4.0](LICENSE).
