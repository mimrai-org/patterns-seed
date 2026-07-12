# Add a contract

1. Identify the runtime boundary, meaning, producer, consumers, owner, and expected compatibility
   lifetime. Verify the entry belongs to this package's existing capability.
2. Choose a public subpath named by the supported operation, event, message, or value rather than its
   current transport or framework.
3. Extend the authoritative schema or protocol definition and derive or regenerate the TypeScript
   type. Do not create a parallel hand-maintained interface.
4. Specify required and optional fields, units, encodings, discriminator behavior, unknown-key policy,
   defaults, and parse failure behavior.
5. Export the smallest supported surface. Keep parsing deterministic and map application models outside
   the contract package.
6. Add positive, negative, boundary, serialization, and compatibility fixtures.
7. Update every producer and consumer through the public entry and run their affected checks.
8. Review bundle size or runtime support when a client with a constrained toolchain imports the schema.

If the contract has a different owner or release cadence from the package, use
`recipes/split-contract-package.md` instead of making the existing package broader.
