# Organize by capability

The primary unit of organization is a user-facing capability, because its UI, data access, state, and
tests usually change together. Global folders by technical type were rejected because they scatter one
change across the codebase and obscure ownership; small applications may reasonably delay this
structure until a second capability appears.
