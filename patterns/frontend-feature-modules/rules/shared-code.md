# Shared code

`src/shared` contains primitives with no feature vocabulary and no dependency on application
composition. Admission always requires a domain-neutral contract and explicit ownership. Evidence that
the boundary is real is either:

1. Multiple independent consumers with the same stable need; or
2. An explicit cross-feature platform or design-system contract, such as accessibility infrastructure,
   that is intentionally shared before its second consumer appears.

Good candidates include visual primitives, generic accessibility behavior, date or number formatting,
and narrow wrappers around browser APIs. A wrapper should expose the capability the application needs,
not mirror an entire vendor library.

Multiple consumers are a strong heuristic, not a mechanical quota. Do not move feature models,
validation policies, API endpoints, cache keys, or workflow decisions into shared merely because
several files use them. Those concepts still need a business owner. Let another
feature consume an explicit public capability or let `app` orchestrate it.

Shared code is not automatically stable. Give it a small public surface, test it as a reusable unit,
and delete exports that no longer have independent consumers. If a shared abstraction accumulates
feature switches, move those policies back to their owners.
