# Operation anatomy

An operation is the smallest application-owned unit that produces one observable outcome for one
request, scheduled job, or consumed message. Name it by intent, using a form such as
`<verb>-<subject>`, rather than by a broad resource or a technical mechanism. If several entry points
must preserve the same invariant, they may call the same operation; if two requests change
independently, they should not be forced through one generic service method.

The complete operation owns these responsibilities:

- `contract.ts` defines technology-neutral input, result, and expected failure values. It contains no
  HTTP request, queue envelope, ORM record, or provider exception.
- `validation.ts` parses untrusted values and reports structural failures in the operation's
  vocabulary. It does not query current state to decide business policy.
- `handler.ts` owns the application outcome. It coordinates current state, business decisions, and
  external effects through explicit dependencies.
- `ports.ts` declares only the volatile outbound capabilities that the handler consumes. No port is
  required for a pure operation or a dependency that is already stable and cheap to use directly.
- `transport.ts` translates one protocol or trigger into the input contract, invokes the operation,
  and maps the result to protocol semantics.
- `adapters/` contains private implementations used only by this operation. A process-wide adapter
  may live outside the slice, but it still implements an operation-owned port and is selected at
  bootstrap. Give that shared runtime path an explicit owner and register it with architecture checks;
  do not hide mapping or policy inside bootstrap.
- `tests/` verifies the same observable outcome and the boundaries that can fail independently.

A minimal, domain-neutral shape for these roles:

```ts
// contract.ts — typed input, result, and expected failures; no thrown business errors
export type RegisterItemInput = { name: string; ownerId: string };
export type RegisterItemResult = { itemId: string };
export type RegisterItemFailure = { kind: "duplicate-name" } | { kind: "owner-not-found" };

// ports.ts — one narrow capability the handler consumes
export interface ItemStore {
  findOwner(ownerId: string): Promise<{ id: string } | null>;
  insertItem(item: { name: string; ownerId: string }): Promise<{ itemId: string } | "duplicate">;
}

// handler.ts — a factory receives ports and returns the executable use case
export const makeRegisterItem =
  (ports: { store: ItemStore }) =>
  async (input: RegisterItemInput): Promise<RegisterItemResult | RegisterItemFailure> => {
    const owner = await ports.store.findOwner(input.ownerId);
    if (!owner) return { kind: "owner-not-found" };
    const inserted = await ports.store.insertItem(input);
    if (inserted === "duplicate") return { kind: "duplicate-name" };
    return { itemId: inserted.itemId };
  };
```

Expected failures are values in the contract; thrown errors are reserved for unexpected faults. A
port may equally be a single function type when one capability suffices.

These are responsibilities, not a required class hierarchy. Keep a small operation in one or two
files until navigation or independent testing warrants a split. Conversely, an operation with rich
policy may contain a local domain model, policies, mapping, and several adapters; that complexity
stays inside its owner rather than becoming an application-wide template.

The slice boundary is not permission to mix all concerns in one function. Transport details stop at
the edge, policy stays testable without a server, and concrete integration details remain behind
ports. Cohesion means related change is nearby, while each responsibility still has a clear owner.

Do not create a generic base handler, repository, validator, or response mapper merely to make every
operation look identical. Consistent naming and lifecycle are useful; forced implementation symmetry
reintroduces horizontal coupling.
