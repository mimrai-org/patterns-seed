# Feature anatomy

A feature directory groups code that changes for the same user-facing capability. A few named files at
its root are public; implementation under `internal/` is private to the feature.

```text
features/<feature>/
├── screen.tsx       example public UI entry
├── routes.tsx       optional public route entry
├── contract.ts      optional stable public input and result types
└── internal/
    ├── api/          request functions, schemas, and cache/query integration
    ├── ui/           feature-owned presentation and interaction
    ├── hooks/        React orchestration that coordinates feature concerns
    ├── model/        state transitions and pure capability rules
    ├── lib/          small private helpers
    ├── assets/       feature-owned assets, when required
    └── __tests__/    cross-file feature tests, when colocation is not enough
```

Create only the segments that earn their existence. A component-specific hook stays beside the
component. A schema used only by one operation stays beside that operation. A `types/` directory is
not a dumping ground for types that have obvious owners.

Public entry files may import `internal/`; external consumers may not. Keep transport mapping at the
edge of `internal/api/`: external payloads are parsed and converted into the
feature's internal representation before the rest of the feature consumes them. Keep state with its
semantic owner. A feature owns its actions, invariants, and state model even when `app` mounts a
process-wide provider or connects several feature contracts; application composition must not absorb
feature policy.

Split a feature when its parts have different reasons to change and can expose coherent public
capabilities. Do not split solely because a directory has many files.
