# Layers

The architecture has three layers ordered by knowledge. Higher layers know how the application is
assembled; lower layers remain reusable because they know less.

## `src/app`

The composition root for the frontend. It owns routes, top-level layouts, providers, global error
boundaries, startup configuration, and orchestration across features. Route files should translate
framework inputs and compose feature capabilities; they should not absorb rules owned by a feature.

## `src/features`

Each child directory owns one user-facing capability. A feature may contain UI, data access, state,
validation, and tests when those elements change with the same capability. Features do not import one
another. If two features must coordinate, `app` composes their public APIs or a new owning capability
is introduced.

## `src/shared`

Domain-agnostic building blocks: visual primitives, generic hooks, formatting utilities, and wrappers
around browser or third-party APIs. Shared code cannot name a feature or encode one feature's policy.
It has no knowledge of `features` or `app`.

## Direction

Allowed local dependencies are:

```text
app      -> features
app      -> shared
features -> shared
```

Everything else crosses the architecture in the wrong direction. Configure path aliases only after
the same rules work with ordinary paths; an alias should improve readability, not conceal ownership.
