# Use public module entry points

Each module exposes one deliberate root API and keeps deeper paths private. Direct implementation
imports were rejected because they make internal layout a system-wide contract; maintaining a facade
adds work but preserves ownership and makes coupling visible.
