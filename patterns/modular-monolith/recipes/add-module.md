# Add a module

1. Name one cohesive capability, its vocabulary, owner, and reason to change.
2. Identify the data and invariants it owns and the outcomes it provides to the rest of the system.
3. Create `src/modules/<module>/index.ts`, `contracts/`, `register.ts`, and the smallest required
   `internal` structure.
4. Define a narrow public API contract and a factory returning `{ api, lifecycle }`; declare any public
   module APIs it requires as explicit factory dependencies.
5. Implement one vertical behavior path and its module-level test.
6. Put persistence and migrations under the module when data is needed.
7. Create the module from `src/host` through its root entry point in dependency order and include its
   startup, readiness, failure cleanup, and shutdown in lifecycle tests.
8. Add architecture rules for the new module identity and public path.
9. Run typecheck, tests, cycle detection, architecture checks, and
   `patterns check --include-tests`.

If the capability has no independent behavior, vocabulary, or data ownership, it may be a component
inside an existing module rather than a new boundary.
