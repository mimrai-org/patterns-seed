# patterns-seed

Curated, domain-agnostic architecture patterns for the
[`patterns`](https://github.com/mimrai-org/patterns) ecosystem.

This repository contains knowledge bundles, not application starters. A pattern describes where code
belongs, which dependencies are allowed, how to extend the architecture, and why its important
decisions were made. Installing one never generates or overwrites application source code.

## Catalog

| Pattern | Use it when | Version |
| --- | --- | --- |
| [`frontend-feature-modules`](patterns/frontend-feature-modules/) | A React application needs feature ownership and one-way dependencies | `0.1.0` |
| [`typescript-hexagonal-service`](patterns/typescript-hexagonal-service/) | A TypeScript service needs a framework-independent core and replaceable adapters | `0.1.0` |
| [`modular-monolith`](patterns/modular-monolith/) | A backend needs strong capability boundaries without distributed-system overhead | `0.1.0` |
| [`monorepo-shared-contracts`](patterns/monorepo-shared-contracts/) | Several applications need shared runtime contracts without sharing business implementations | `0.1.0` |
| [`swappable-repository-adapters`](patterns/swappable-repository-adapters/) | One persistence capability needs conforming adapters and a reversible replacement path | `0.1.0` |
| [`vertical-slice-use-case`](patterns/vertical-slice-use-case/) | Backend operations should evolve as cohesive slices instead of global technical layers | `0.1.0` |

## Install

Each directory under `patterns/` is an independent bundle:

```sh
patterns add mimrai-org/patterns-seed/patterns/frontend-feature-modules
patterns add mimrai-org/patterns-seed/patterns/typescript-hexagonal-service
patterns add mimrai-org/patterns-seed/patterns/modular-monolith
patterns add mimrai-org/patterns-seed/patterns/monorepo-shared-contracts
patterns add mimrai-org/patterns-seed/patterns/swappable-repository-adapters
patterns add mimrai-org/patterns-seed/patterns/vertical-slice-use-case
```

These commands follow the repository's default branch. Once a release tag exists, pin a production
install by appending the `<pattern-name>-v<version>` tag, for example:

```sh
patterns add mimrai-org/patterns-seed/patterns/frontend-feature-modules#frontend-feature-modules-v0.1.0
```

The catalog at [patterns.directory](https://patterns.directory) is only the discovery index. Git
remains the source of truth and the distribution channel.

## License

Original pattern content is licensed under
[Creative Commons Attribution 4.0 International](LICENSE). Every bundle carries its own `LICENSE` so
the terms and preferred attribution remain present after a Git-native subdirectory installation.
Linked third-party references are not redistributed and remain under their respective licenses.

## Editorial standard

Every seed in this repository must:

- Be `shareable`: no company, product, team, or business-specific entity names.
- Teach one primary architectural decision rather than bundle an entire product stack.
- State when it fits, when it does not, and what complexity it introduces.
- Describe responsibilities and dependency direction, not a line-by-line project inventory.
- Include machine-checkable boundaries where the `patterns` glob model can express them.
- Use narrow roles and interfaces; apply SOLID principles as design constraints, not ceremony.
- Keep recipes as templates with role placeholders such as `<feature>` and `<use-case>`.
- Use original wording and implementation-independent examples. External projects are research
  references, never sources to copy wholesale.

## Validate

From this repository, validate every bundle before publishing:

```sh
for pattern in patterns/*; do patterns validate "$pattern"; done
```

Also install each bundle into a temporary project and run `patterns check --include-tests` against
representative allowed and forbidden import graphs. Schema validation proves integrity; it does not
replace editorial or architecture review.
