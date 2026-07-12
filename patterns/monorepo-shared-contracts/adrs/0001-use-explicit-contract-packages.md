# Use explicit contract packages

Cross-application contracts live in namespaced, capability-sized workspace packages with declared
dependencies and exports. Application folders, root aliases, and a global `shared-types` bucket were
rejected because they hide ownership and package graph edges. The cost is package metadata and task
configuration; the benefit is a reviewable dependency, public surface, and affected-consumer graph.
