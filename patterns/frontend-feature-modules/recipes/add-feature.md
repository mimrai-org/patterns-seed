# Add a feature

Use this recipe when a new user-facing capability has an independent reason to change.

1. Name the capability in the product's user or domain language, avoiding technical buckets, and create
   `src/features/<feature>/`.
2. Write the smallest named public entry first: usually `screen.tsx`, `routes.tsx`, or a command file
   and its behavior test.
3. Put implementation under `internal/` and add segments only as responsibilities appear:
   - `api/` for external data access and mapping.
   - `model/` for state transitions or pure rules.
   - `ui/` and `hooks/` for React behavior.
   - `lib/` for private helpers.
4. Keep stable input or result types in a root `contract.ts` only when consumers need them.
5. Compose the named entry from a route, layout, or orchestrator under `src/app`.
6. Verify the feature imports only itself and `src/shared`.
7. Add import-boundary coverage for any new alias or path form.
8. Run typecheck, tests, module-aware import checks, and `patterns check --include-tests`.

If the new feature immediately needs another feature's internals, stop and resolve ownership. Pass a
value or callback from `app`, consume the other feature's deliberate public capability through app
composition, or redefine the capability boundary.
