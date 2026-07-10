# Testing and release model

Compilation proves that applications agree with the contract source in the current checkout. It does
not prove that independently deployed versions can communicate.

## Test layers

1. **Contract unit tests** exercise accepted values, rejected values, serialization, defaults, and
   boundary cases inside the contract package.
2. **Compatibility fixtures** preserve representative old and candidate payloads for the supported
   compatibility window. Fixtures contain synthetic, non-sensitive serialized data.
3. **Cross-version reader tests** execute the candidate reader against supported old payloads and the
   previous supported reader or consumer artifact against candidate producer output.
4. **Producer tests** prove emitted values satisfy the exported contract and rejected input fails at
   the system boundary.
5. **Consumer tests** prove supported contract versions map into application-owned models and that
   unknown or invalid input follows an explicit failure path.
6. **Graph tests** reject undeclared dependencies, deep imports, app-to-app edges, reverse edges, and
   workspace cycles.

Tests consume public exports. A contract package test must not import an application to obtain a
fixture, and an application test must not deep-import contract internals. Test-only dependencies are
still architecture dependencies.

A fixture does not execute an old reader by itself. Obtain that reader from an immutable package
version, release artifact, tag, or pinned consumer commit, and run it without rebuilding against the
candidate contract. The minimum release matrix is:

| Reader | Payload | Proves |
| --- | --- | --- |
| Candidate | Oldest supported fixture | The new reader remains compatible with retained old data |
| Previous supported artifact | Candidate writer fixture | A new writer does not strand a deployed or rollback reader |
| Candidate | Candidate fixture | The new pair agrees on current behavior |

For a private source-exported package, CI may check out the previous consumer commit or preserve a
built parser artifact. Record the exact version or commit in test output so the compatibility claim is
reproducible.

## Change rollout

For applications that may deploy separately, use expand–migrate–contract:

1. Expand the contract with a backward-compatible representation or a parallel version.
2. Pass the cross-version reader matrix against the oldest supported reader and payload.
3. Release consumers that can read both old and new values.
4. Release producers that emit the new value.
5. Observe the compatibility window and remove legacy use only after deployed consumers are known to
   be migrated.
6. Remove the old representation in a later, explicit breaking change.

Adding a required field, narrowing an accepted value, changing a discriminator, or altering parser
semantics can be breaking even if TypeScript compiles after an atomic source edit. Optional fields are
not automatically safe when consumers use exhaustive checks or reject unknown keys; test actual parser
policies.

## Task graph

Each package owns its build, typecheck, lint, and test scripts. The root task runner delegates across
the declared workspace graph. Build tasks wait for dependency builds when consuming compiled outputs;
affected checks include all dependents of a changed contract package. Cache declared files only when a
task produces them.

TypeScript project references are an optional compilation optimization, not the ownership graph. If
used, check that references agree with package dependencies so build order and architecture cannot
diverge.
