# Enforce one-way layers

Dependencies flow from application composition to features to shared primitives and are checked by
complementary deterministic controls. Manifest boundaries enforce layer direction and private app
imports, and a manifest isolation rule rejects cross-feature imports; a module-aware linter or
architecture test adds root-qualified feature identity and resolver-aware coverage. Convention-only
boundaries were rejected because a single convenient import creates cycles and makes later violations
easier; the cost is maintaining both checks alongside path changes.
