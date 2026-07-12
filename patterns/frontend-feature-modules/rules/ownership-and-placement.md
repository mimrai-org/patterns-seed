# Ownership and placement

Place code with the capability that owns its reason to change.

- If a component changes when one capability changes, keep it in that feature even if it looks
  reusable.
- If a request, schema, cache key, or state transition serves one capability, that feature owns it.
- If code assembles routes, providers, or several features, `app` owns it.
- If a primitive is domain-agnostic and has independent consumers, `shared` may own it.

Do not choose a folder from the symbol's technical type alone. A feature-specific hook does not belong
in a global `hooks/` directory, and a capability-owned request does not belong in a global `services/`
directory.

Prefer colocation before abstraction. A duplicate with one owner is easier to change than a shared
abstraction with several hidden policies. Promote code only after its stable responsibility is visible.

Names should express capability and role. Avoid catch-all directories or symbols such as `common`,
`misc`, `helpers`, and `manager`; they hide responsibility rather than define it.
