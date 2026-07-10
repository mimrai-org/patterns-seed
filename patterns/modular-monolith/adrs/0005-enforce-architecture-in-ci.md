# Enforce architecture in CI

Import direction, public entry points, and module cycles are checked automatically. Convention-only
boundaries were rejected because in-process convenience makes erosion cheap and gradual; deterministic
checks require resolver maintenance and module-aware rules but keep violations visible at review time.
