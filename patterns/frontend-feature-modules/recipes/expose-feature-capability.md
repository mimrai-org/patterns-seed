# Expose a feature capability

Use this recipe when application composition needs behavior owned by a feature.

1. Describe the smallest consumer need without naming the feature's internal implementation.
2. Choose a narrow surface: a component, hook, command function, callback contract, or stable type.
3. Map vendor and transport types to feature-owned types before they cross the boundary.
4. Add a small, purpose-named file at `src/features/<feature>/` that exposes the surface while keeping
   its implementation under `internal/`.
5. Import it from `src/app`; do not expose a deep path for convenience.
6. Add or update a behavior-level test at the public boundary.
7. Check that no private state container, raw response, or incidental helper leaked into the entry.

If several unrelated exports are needed together, reconsider whether the consumer is doing feature
work that belongs inside the feature. If another feature wants the export, keep the actual composition
in `app` rather than creating a lateral dependency.
