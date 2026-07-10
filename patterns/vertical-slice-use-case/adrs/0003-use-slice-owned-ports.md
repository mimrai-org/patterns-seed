# Use slice-owned ports

Each handler defines only the outbound capabilities it consumes, and concrete adapters implement those
contracts. Application-wide repository and provider interfaces were rejected because unrelated slices
rarely need identical semantics; local ports may repeat small shapes, but they preserve interface
segregation and let each operation evolve independently.
