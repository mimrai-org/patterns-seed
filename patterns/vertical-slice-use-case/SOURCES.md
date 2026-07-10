# Research sources

This bundle is an original, framework-neutral TypeScript synthesis. No source code, diagram, folder
tree, example domain, or prose from the references was copied.

Sources were reviewed on 2026-07-10:

- [Jimmy Bogard, “Vertical Slice Architecture”](https://www.jimmybogard.com/vertical-slice-architecture/).
  Consulted as the primary description of organizing around distinct requests, maximizing cohesion
  inside a slice, minimizing coupling between slices, and allowing each slice to adopt only the
  modeling complexity it needs. No reuse license was identified on the article, so it is linked for
  conceptual provenance only.
- [`jbogard/ContosoUniversityDotNetCore` at `209b576`](https://github.com/jbogard/ContosoUniversityDotNetCore/tree/209b576f31d3482feba9b184f6851bb3182a3f10).
  Consulted as the author's reference implementation of feature folders, vertical slices, CQRS, and
  direct application integration. The project is
  [MIT licensed](https://github.com/jbogard/ContosoUniversityDotNetCore/blob/209b576f31d3482feba9b184f6851bb3182a3f10/LICENSE).
- [`gm50x/nestjs-cqrs-ddd` at `4365908`](https://github.com/gm50x/nestjs-cqrs-ddd/tree/4365908e27185ff53bfc07cbebbe21a218b14f38).
  Consulted to compare a TypeScript/NestJS implementation that groups domain contexts as slices and
  treats internal layers as optional. The repository has no `LICENSE` file and its
  [`package.json` declares `UNLICENSED`](https://github.com/gm50x/nestjs-cqrs-ddd/blob/4365908e27185ff53bfc07cbebbe21a218b14f38/package.json),
  so no code, structure, or text was reused.
- [NestJS CQRS documentation at `2f9d939`](https://github.com/nestjs/docs.nestjs.com/blob/2f9d9394aa71ea666816c8a3475fbc6d165962c6/content/recipes/cqrs.md).
  Consulted only to distinguish command/query semantics from the optional command, query, and event
  buses offered by a framework. The documentation repository is
  [MIT licensed](https://github.com/nestjs/docs.nestjs.com/blob/2f9d9394aa71ea666816c8a3475fbc6d165962c6/LICENSE).

Mediators, buses, separate read stores, event sourcing, DDD tactical patterns, ORMs, dependency
injection frameworks, and message brokers are deliberately excluded from the core contract. They are
independent choices that a slice may adopt when its requirements justify their cost.
