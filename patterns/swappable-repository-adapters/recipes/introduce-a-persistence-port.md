# Introduce a persistence port

Use this recipe to extract a justified seam around an existing direct persistence dependency without
changing behavior.

1. Select one cohesive consuming capability and list the exact storage operations it currently uses.
2. Capture current observable behavior with characterization and integration tests: absence, conflicts,
   ordering, concurrency, transaction scope, mapping, and expected failures.
3. Define an application-owned repository port with only those required operations and technology-neutral
   values. Do not copy the entire client or ORM API.
4. Move record and schema types behind a mapper in a `current` adapter. Translate vendor failures to the
   port's documented outcomes.
5. Make the current behavior tests the first fixture of a shared contract suite. Preserve behavior before
   attempting to improve it.
6. Change consumers to receive the port explicitly. Remove their storage-client, record, decorator, query,
   transaction, and vendor-error imports.
7. Select and construct the current adapter in bootstrap. Keep the same store authoritative.
8. Add architecture enforcement for application-to-persistence imports and adapter selection.
9. Run application, contract, adapter integration, end-to-end, and architecture tests.

Stop if the proposed port remains a universal data API or consumers need arbitrary query-builder access.
Split the capability, introduce a dedicated read projection, or keep direct data access until a coherent
contract exists. The first extraction should be behavior-preserving; evolve semantics in a separate,
reviewable change.
