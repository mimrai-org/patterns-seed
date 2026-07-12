# Module encapsulation

Code outside `src/modules/<module>` imports that module only through
`src/modules/<module>/index.ts` or an equivalent package root.

The root entry point exports a deliberate public surface. It must not re-export internal entities,
repositories, ORM models, handlers, or framework details merely to make an import convenient.

Enforce encapsulation with package `exports`, resolver-aware linting, or an architecture test. The test
must capture both source and target identities as `<normalized deployable root, module>`: a module may
use its own internal paths, while another module or an equal-slug module in a different application may
not. The manifest's `isolations` rule catches cross-module imports of `internal/**` within one source
tree. It keys identity by directory name alone, so a dedicated architecture test is still required for
two cases: equal module slugs under different deployable roots, and deep imports of another module's
`contracts/` or `register.ts` from non-internal files. Do not replace either check with a broad
module-to-module prohibition, which would also reject a module's own internal imports.

Temporary exceptions need an owner, reason, and removal condition. Re-exporting a private symbol from
the public entry point to silence the check is an API decision and must receive the same review as any
other public contract.
