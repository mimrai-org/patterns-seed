# Use explicit entry files

Every feature exposes a few small named files at its root and treats `internal/` as private. Direct
internal imports and a catch-all directory barrel were rejected because they turn file layout into a
public contract, make cycles easier, and can weaken tree-shaking; the chosen approach requires
deliberate entry-file maintenance and can reveal when a feature has grown too broad.
