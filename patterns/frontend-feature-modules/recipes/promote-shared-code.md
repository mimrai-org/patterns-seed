# Promote code to shared

Use this recipe after a primitive has multiple independent consumers or an explicitly owned,
domain-neutral platform contract justifies sharing it earlier.

1. List the consumers and behavior each one needs, or record the stable cross-feature contract and its
   owner.
2. Remove feature vocabulary and policy from the candidate abstraction.
3. Define the smallest stable API that satisfies the common behavior; leave variations with their
   feature owners.
4. Move the primitive to a specific role under `src/shared`, not to `shared/common` or `shared/misc`.
5. Move or add reusable tests that depend on no feature fixtures.
6. Update consumers through the shared public path.
7. Confirm `shared` imports neither `features` nor `app`.
8. Delete the old copies only after all consumers pass their behavior tests.

If the result needs feature switches, business entity names, or callbacks tailored to one consumer,
keep the implementations separate. Similar syntax is not sufficient evidence of shared responsibility.
