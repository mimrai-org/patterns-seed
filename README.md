# Seed Architecture Patterns

Six curated, domain-agnostic architecture guides for the
[`patterns`](https://github.com/mimrai-org/patterns) ecosystem. Each pattern explains where code
belongs, which dependencies are allowed, how to extend the architecture, and why its important
decisions were made.

These are knowledge bundles, not application starters. Installing one never generates or overwrites
application source code.

## Choose a pattern

Start with the decision you need to make:

```text
Where is the architectural pressure?
├── A React app is hard to navigate by capability
│   └── Organize a React App by Feature
├── Backend business logic is coupled to frameworks or vendors
│   └── Build a Framework-Independent TypeScript Service
├── One backend needs strong internal capability boundaries
│   └── Structure One Backend with Strong Module Boundaries
├── Several workspace apps duplicate the same data contracts
│   └── Share Type-Safe Contracts Across Monorepo Apps
├── A database must be isolated, tested, or replaced safely
│   └── Replace Database Adapters Safely
└── One backend change crosses controllers, services, and repositories
    └── Organize Backend Features by Use Case
```

## Catalog

| Human guide | Stable Pattern ID | Best for | Stack | Version |
| --- | --- | --- | --- | --- |
| [Organize a React App by Feature](patterns/frontend-feature-modules/) | `frontend-feature-modules` | Keeping UI, state, data access, and tests with the feature that owns them | TypeScript · React · Browser | `0.2.0` |
| [Build a Framework-Independent TypeScript Service](patterns/typescript-hexagonal-service/) | `typescript-hexagonal-service` | Keeping business logic independent from HTTP, databases, queues, and vendors | TypeScript · Node.js · Framework-independent | `0.2.0` |
| [Structure One Backend with Strong Module Boundaries](patterns/modular-monolith/) | `modular-monolith` | Giving capability modules explicit APIs and data ownership in one deployable | TypeScript · Node.js · Framework-independent | `0.2.0` |
| [Share Type-Safe Contracts Across Monorepo Apps](patterns/monorepo-shared-contracts/) | `monorepo-shared-contracts` | Sharing validated data shapes without sharing application behavior | TypeScript · Browser/Node.js/Worker/Mobile | `0.2.0` |
| [Replace Database Adapters Safely](patterns/swappable-repository-adapters/) | `swappable-repository-adapters` | Testing equivalent persistence adapters and planning a reversible migration | TypeScript · Node.js · Framework-independent | `0.2.0` |
| [Organize Backend Features by Use Case](patterns/vertical-slice-use-case/) | `vertical-slice-use-case` | Keeping each request, job, or message with the code needed for its outcome | TypeScript · Node.js · Framework-independent | `0.2.0` |

The stable Pattern ID is the install identity and directory name. Human titles explain the outcome;
they do not replace published IDs.

## How the patterns relate

The guides make decisions at different scales and can be combined selectively:

```text
TypeScript workspace
├── Shared contract packages connect applications
├── React application
│   └── Feature modules organize user-facing capabilities
└── Backend deployable
    ├── Capability modules create strong system boundaries
    └── Inside a service or module
        ├── Vertical slices organize individual operations
        ├── Hexagonal boundaries isolate external technology
        └── Replaceable adapters isolate persistence when needed
```

This is a map, not a recommendation to install every pattern. For example, vertical slices and
hexagonal architecture answer different questions and can coexist, while replaceable database
adapters add value only at a real persistence volatility or migration boundary.

## Install

Each directory under `patterns/` is an independent bundle. Pin a reviewed release with its matching
tag:

```sh
patterns add mimrai-org/patterns-seed/patterns/frontend-feature-modules#frontend-feature-modules-v0.2.0
patterns add mimrai-org/patterns-seed/patterns/typescript-hexagonal-service#typescript-hexagonal-service-v0.2.0
patterns add mimrai-org/patterns-seed/patterns/modular-monolith#modular-monolith-v0.2.0
patterns add mimrai-org/patterns-seed/patterns/monorepo-shared-contracts#monorepo-shared-contracts-v0.2.0
patterns add mimrai-org/patterns-seed/patterns/swappable-repository-adapters#swappable-repository-adapters-v0.2.0
patterns add mimrai-org/patterns-seed/patterns/vertical-slice-use-case#vertical-slice-use-case-v0.2.0
```

Omit the `#<pattern-name>-v<version>` suffix only when intentionally following the repository's
default branch. The catalog at [patterns.directory](https://patterns.directory) is the discovery
index; Git remains the source of truth and the distribution channel.

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
