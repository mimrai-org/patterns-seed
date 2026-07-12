# Align data ownership with modules

A module exclusively owns the meaning and mutation of its persistence model even when the physical
database is shared. Cross-module table access was rejected because it bypasses behavior and prevents
independent evolution; public queries or replicated projections introduce explicit call or consistency
costs instead.
