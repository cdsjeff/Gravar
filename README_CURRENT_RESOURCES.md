# Gravar v0.6.198 Current Resources

This package contains the current standalone Gravar build and supporting repository files.

- `Gravar_v0.6.198_Default_Symbols_Migration.html` — named standalone HTML build.
- `index.html` — identical copy for GitHub Pages/static hosting.
- `gravar-shared-symbols.json` — expanded 44-row built-in default Symbols palette supplied for Gravar.
- `CHANGELOG.md` — version-organized revision history.
- `CHANGELOG_BY_DATE.md` — date-organized revision history.
- `ARCHITECTURE_AUDIT_v0.6.197.md` — deep modularity/message-passing audit that remains the architectural reference for this incremental release.
- `SHA256.txt` — checksums for packaged resources.

## v0.6.198 focus

This release fixes default-Symbol persistence across project generations. `XSymbolDefaults` now owns the built-in expanded palette and recognizes exact historical Gravar built-in palettes saved by older projects. Those legacy defaults migrate to the current 44-row palette automatically, while genuinely customized project Symbols are preserved.

Symbols persistence now participates in the modular `XProjectState` adapter system introduced in v0.6.197; legacy top-level Symbols fields remain in saved JSON for backward compatibility.
