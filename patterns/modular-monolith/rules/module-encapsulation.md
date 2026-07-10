# Module encapsulation

Code outside `src/modules/<module>` imports that module only through
`src/modules/<module>/index.ts` or an equivalent package root.

The root entry point exports a deliberate public surface. It must not re-export internal entities,
repositories, ORM models, handlers, or framework details merely to make an import convenient.

Enforce encapsulation with package `exports`, resolver-aware linting, or an architecture test. The test
must capture both source and target identities as `<normalized deployable root, module>`: a module may
use its own internal paths, while another module or an equal-slug module in a different application may
not. The basic `patterns` glob checker cannot express that identity comparison, so do not replace the
dedicated rule with a broad module-to-module prohibition.

Temporary exceptions need an owner, reason, and removal condition. Re-exporting a private symbol from
the public entry point to silence the check is an API decision and must receive the same review as any
other public contract.
