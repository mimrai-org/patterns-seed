# Add a module use case

1. Confirm which module owns the requested outcome.
2. Define a technology-neutral input, result, and expected failures inside that module.
3. Place invariants in its domain code when they must hold across entry points.
4. Add application orchestration and define narrow outbound contracts for external effects.
5. Implement adapters behind those contracts without exposing them publicly.
6. Test the behavior at the use-case or module-facade boundary.
7. Expose the operation only if an outside consumer exists; otherwise keep it internal.
8. Verify no host or sibling internals were imported.

When no existing module clearly owns the outcome, resolve ownership before adding a shared service or
host policy. Create an explicit coordinating capability module when the workflow itself needs an owner.
Ambiguous placement is an architecture signal, not a folder-selection problem.
