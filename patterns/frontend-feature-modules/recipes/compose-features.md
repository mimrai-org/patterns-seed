# Compose features

Use this recipe when one route or workflow needs capabilities owned by more than one feature.

1. Keep each feature's state model, actions, and invariants inside that feature.
2. Expose only the values, callbacks, components, or commands required for composition through named
   root entry files.
3. Create a thin route, layout, or orchestrator under `src/app` that receives those public contracts.
4. Pass data and callbacks explicitly. Do not import one feature from another or move their policy into
   the app layer.
5. If shared runtime state is required, let `app` mount the provider while the owning feature retains
   the state contract and behavior.
6. Test each feature independently, then add one app-level integration test for the composition.
7. Run module-aware import checks and `patterns check --include-tests`.

If the orchestrator accumulates decisions rather than simple sequencing and wiring, the workflow needs
an owning feature of its own or the existing feature boundary is incorrect.
