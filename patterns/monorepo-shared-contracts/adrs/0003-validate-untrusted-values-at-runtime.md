# Validate untrusted values at runtime

Values crossing process, network, queue, file, storage, or user boundaries are parsed by a runtime
contract before application logic uses them. TypeScript types are derived or generated from that same
authority. Type-only assertions were rejected for untrusted data because they disappear at runtime.
The cost is parser execution and dependency weight; narrow exports and generated protocols remain
valid alternatives for constrained or non-TypeScript consumers.
