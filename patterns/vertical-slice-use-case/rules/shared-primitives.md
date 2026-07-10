# Shared primitives

`src/shared` contains stable, domain-neutral primitives with multiple independent consumers and no
dependency on an operation or bootstrap. Appropriate examples include a narrow result type, clock
contract, tracing context, or small language-level utility whose semantics are the same for every
consumer.

Do not place operation inputs, business errors, persistence models, provider wrappers, generic
repositories, coordinators, or domain vocabulary in shared. Cross-capability business contracts need
an explicit capability owner or enclosing module, not a neutral-sounding folder.

Before promotion, require:

1. At least two independent consumers with the same semantics, not merely similar syntax.
2. A responsibility that can be named without either operation's vocabulary.
3. A stable contract smaller than the duplicated implementation.
4. An owner for compatibility, tests, and removal.

Move the minimum primitive and leave operation-specific policy behind. Shared code must not inspect
operation names, route by a type switch, or grow optional parameters for individual slices. When
consumers begin changing it in different directions, duplicate the small behavior back into its owners
or introduce a deliberately named higher-level capability.

Test utilities follow the same rule. A shared fixture that constructs business state for several
slices is often hidden coupling; prefer operation-owned builders and share only neutral harnesses.
