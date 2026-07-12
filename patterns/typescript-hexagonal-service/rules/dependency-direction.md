# Dependency direction

Source dependencies point toward application policy:

```text
bootstrap -> adapters -> application -> domain
```

Enforce these prohibitions:

- Domain imports neither application, adapters, nor bootstrap.
- Application imports neither adapters nor bootstrap.
- Inbound and outbound adapters do not import one another.
- Adapters do not import bootstrap.

The domain should also remain free of framework decorators and vendor types even when those imports
would not create a local graph edge. Use lint rules, dependency analysis, and focused architecture
tests alongside the `patterns.yaml` boundaries.

In a monorepo, qualify every layer by its normalized service or workspace-package root. The manifest's
leading globs enforce forbidden directions across nested `src` trees, but they cannot distinguish an
allowed inward edge inside one service from an accidental source import into another service. Add a
resolver-aware rule that rejects cross-root source imports even when their layer direction appears
valid. Deliberate reuse crosses a declared package public export or protocol boundary. Include a
negative fixture with equal layer paths under two service roots.

When a forbidden dependency seems convenient, move translation outward, inject a narrow port, or move
the policy inward. Do not introduce a re-export or global service locator to conceal the edge.
