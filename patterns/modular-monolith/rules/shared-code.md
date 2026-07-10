# Shared code

`src/shared` contains stable, domain-agnostic primitives with multiple independent consumers. It has
no dependency on a module or the host.

Appropriate candidates include narrow logging or tracing contracts, language-level result types, and
small infrastructure-neutral utilities. Business entities, workflows, persistence abstractions, and
module-specific errors do not belong in shared.

Prefer small duplication over premature cross-module abstraction. Promote code only when its neutral
responsibility can be named without either consumer. If shared code accumulates switches for modules,
move the behavior back to its owners or create an explicit capability module.

A shared kernel containing domain concepts is a separate, high-coupling decision. Keep it tiny, assign
an owner, and require coordinated changes; do not let it emerge accidentally from a generic `common`
directory.
