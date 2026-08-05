# Changelog

All notable changes to `truecopy-action` are documented here. The action follows
the moving-major-tag convention: pin `@v1` to track patches, or `@<full-sha>` for
an airtight supply chain.

## [Unreleased]

## [1.0.3]

### Changed
- **Default `truecopy-ref` moved `v0.10.1` → `v0.10.3`.** truecopy 0.10.3 carries its redstamp dependency forward, `0.7.3` → `0.7.5`: a proven bypass of redstamp's deterministic black gate via PowerShell's `-ArgumentList` array form (`'-ExecutionPolicy','Bypass',...`), which scored green while the equivalent space-separated command correctly scored black (askalf/redstamp#124). Since redstamp is the engine every `verify`/`scan` call in this action runs through, a consumer on the old default could miss a skill or MCP server using that evasion shape. No input contract change — a tag, branch, or SHA all still work the same way.

*(The entry below was left over from before v1.0.0 shipped — corrected here since it still claimed the default was `v0.8.0`, three releases stale.)*

### Changed (v1.0.0)
- **Renamed `canon-action` → `truecopy-action`**, tracking the [`canon` → `truecopy`](https://github.com/askalf/truecopy) rename. The old `askalf/canon-action` name still resolves via GitHub's rename redirect; the `canon-ref` input is now `truecopy-ref`.

### Fixed (v1.0.0)
- **`canon.lock` back-compat now works from the action.** The action previously always passed `--lock truecopy.lock`, which defeated truecopy's built-in fallback to a pre-rename `canon.lock` — a repo pinned before the rename would fail the gate looking for a file it never had. The `lock` input now defaults to empty and only passes `--lock` when set to a non-default path, so truecopy resolves `truecopy.lock` → `canon.lock` itself.

### Added (v1.0.0)
- `SECURITY.md`, `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`.
- OpenSSF Scorecard + CodeQL workflows (pinned by SHA) and a Scorecard badge.
- Dependabot (`github-actions`) to keep the pinned `actions/checkout` SHA fresh.
- This changelog.

## [0.1.0] — 2026-07-04

### Added
- Initial release: composite action wrapping the engine (then `canon`). `verify` gates the lock for drift / poisoning / publisher trust; `scan` poison-scans manifests, skill directories, or a cloned marketplace repo. Zero third-party action dependencies; the engine is installed at the ref you pin.

[Unreleased]: https://github.com/askalf/truecopy-action/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/askalf/truecopy-action/releases/tag/v0.1.0
