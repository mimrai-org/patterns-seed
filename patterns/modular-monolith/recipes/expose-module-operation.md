# Expose a module operation

1. Describe the consumer's smallest required capability in the owning module's vocabulary.
2. Define stable input, result, and expected failure types under `contracts/`.
3. Implement the operation on the API object returned by the module factory so it delegates to the
   initialized private application behavior.
4. Export the API contract and factory deliberately from the module's root `index.ts`; do not export an
   active process-global instance.
5. Keep domain entities, persistence records, framework objects, and private exceptions behind the
   boundary.
6. Add consumer-facing contract tests.
7. Import the API contract through the module root, inject its initialized instance during composition,
   and update architecture checks if a new path form is introduced.

Do not expose a generic container, repository, or query mechanism. Each public operation is a supported
capability whose compatibility the module now owns.
