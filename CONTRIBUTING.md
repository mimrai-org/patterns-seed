# Contributing

## Pattern shape

Each contribution lives at `patterns/<slug>/` and contains:

```text
patterns.yaml
README.md
AGENTS.md
SOURCES.md
LICENSE
structure/
rules/
recipes/
adrs/
```

`patterns.yaml` is the only structured file. Its rich index must point to every architecture document
that an agent may need. `SOURCES.md` records research provenance but is not part of the agent routing
index.

## Review checklist

- The problem and adoption threshold are clear.
- The pattern remains useful outside the author's original domain.
- Responsibilities have one owner and dependencies point in one direction.
- Interfaces are introduced at volatile boundaries, not around every function.
- The README names costs, failure modes, and simpler alternatives.
- Recipes add one architectural unit without inventing a concrete business feature.
- ADRs capture only consequential, non-obvious trade-offs.
- Boundaries are enforceable and do not accidentally forbid valid imports.
- Identity-aware checks include the owning root when the same feature, module, slice, or adapter slug
  can appear more than once in a monorepo.
- Source references and licenses were checked at a specific revision before publication.
- A bundle-local `LICENSE` contains the repository's CC BY 4.0 notice and preferred attribution.
- `patterns validate` and a local installation pass.

Original pattern documentation is licensed under CC BY 4.0. Do not change that policy inside an
individual contribution. Third-party references remain under their own terms and must not be copied
into a bundle unless their license and attribution requirements are handled explicitly.

## Versioning

Version each manifest independently with semantic versioning. A release tag uses
`<pattern-name>-v<version>` so a consumer can install a reproducible subdirectory ref:

```text
mimrai-org/patterns-seed/patterns/<pattern-name>#<pattern-name>-v<version>
```

- Patch: editorial clarification or recipe correction that does not change required structure.
- Minor: backward-compatible guidance, optional structure, or a new recipe.
- Major: a changed dependency direction, required layout, public boundary, or removed guidance that
  existing consumers rely on.

The manifest version, catalog table, release tag, and published index entry must agree.
