# Share contracts, not business implementation

Contract packages contain boundary vocabulary, validation, and serialization metadata only.
Applications retain workflows, policy, persistence, clients, framework adapters, and composition.
Sharing those implementations was rejected because it turns a stable integration boundary into hidden
runtime coupling. Some mapping is duplicated at application edges; that duplication protects separate
ownership and reasons to change.
