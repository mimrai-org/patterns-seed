# Compatibility and ownership

Every contract package has one accountable owner and a visible set of producing and consuming
applications. Shared use does not mean shared ambiguity: the owner classifies changes, coordinates the
rollout, and decides when compatibility evidence is sufficient.

Before changing a contract, identify:

- All runtime producers and consumers, including jobs, scripts, and delayed queue readers.
- Whether applications deploy atomically or independently.
- The oldest supported deployed version or retained message.
- The release tag, package version, artifact, or consumer commit that supplies the previous reader for
  executable compatibility checks.
- Parser policies for unknown keys, defaults, discriminators, and exhaustive matching.
- The deprecation window and observable signal used to remove old behavior.

Usually compatible changes are additive and tolerated by existing parsers. Potentially breaking
changes include adding a required field, removing or renaming a field, narrowing accepted values,
changing units or meaning, changing a discriminator, and changing default, coercion, or transform
behavior. Classify by observed runtime behavior, not only by what the current TypeScript checkout
accepts.

Use expand–migrate–contract for separate deployments. Add a compatible representation or parallel
version, migrate readers, switch writers, observe, then remove legacy behavior later. When two versions
must coexist, expose them explicitly rather than making one schema depend on application state.

Compatibility evidence is directional. Execute the candidate reader against old payload fixtures and
execute the previous supported reader or consumer artifact against candidate writer output. Do not
recompile the previous reader against candidate source: that tests new code under an old label rather
than the code that can still be deployed or rolled back.

Private workspace versions still need change records and compatibility tests. If a package is
published, semantic versioning and release notes additionally communicate the contract; they do not
replace a safe deployment sequence.
