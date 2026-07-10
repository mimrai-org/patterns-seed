# Promote a shared primitive

1. Identify at least two independent operation owners with the same semantics and change cadence.
2. Describe the candidate without either operation's vocabulary. If that is impossible, leave it
   local or assign it to an explicit capability owner.
3. Compare the future coupling cost with the size of the duplication. Prefer duplication when the
   abstraction needs flags, callbacks, or union types for individual slices.
4. Extract the smallest stable contract and implementation under `src/shared`; do not move surrounding
   operation policy with it.
5. Give the primitive one owner, focused tests, compatibility expectations, and a removal path.
6. Migrate one consumer at a time and verify its observable behavior remains unchanged.
7. Add or retain boundaries proving shared imports neither operations nor bootstrap.
8. Review the primitive when a consumer requests a special case. Split it before a neutral utility
   becomes a hidden workflow.

Do not promote business input or result types merely to let one slice import another indirectly.
