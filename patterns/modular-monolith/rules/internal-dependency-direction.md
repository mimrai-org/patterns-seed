# Internal dependency direction

Within a layered module, dependencies point toward policy:

```text
register/adapters -> application -> domain
```

- Domain code imports neither application nor adapters.
- Application code imports domain and application-owned ports, never adapters.
- Adapters implement application ports and translate external models.
- `register.ts` selects concrete implementations and exposes an opaque module registration surface.

A simple module does not need empty domain and application directories. It still preserves the same
direction: policy does not import a transport, database, or registration mechanism.

Apply architecture checks inside each module. Do not use the module's public `index.ts` as an internal
shortcut because it creates cycles and makes private code depend on its external presentation.
