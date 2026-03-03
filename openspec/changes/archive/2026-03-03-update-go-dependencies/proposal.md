## Why

The project targets Go 1.17 (August 2021) and pins `golang.org/x/crypto` to a March 2022 pre-module snapshot. Both are over 4 years old and miss important security patches, performance improvements, and modern Go idioms. Updating now reduces CVE exposure and unblocks use of language features available since Go 1.22+.

## What Changes

- Bump `go` directive in `go.mod` from `1.17` to `1.26.0`
- Update `golang.org/x/crypto` to the latest tagged release (semver)
- Run `go mod tidy` to reconcile indirect dependencies
- Modernize code in `crypto/crypto.go`, `webhooks/webhooks.go`, and `example/main.go` to use current Go idioms (error wrapping, `errors.Join`, `any` type, range-over-int, `log/slog`, etc.)
- **BREAKING**: minimum Go version required by consumers rises from 1.17 to 1.26

## Capabilities

### New Capabilities
- `dependency-update`: Covers the Go version bump, dependency upgrades, and `go mod tidy` reconciliation
- `code-modernization`: Covers idiomatic Go updates across all source files (error handling, naming, stdlib usage)

### Modified Capabilities

(none — no existing specs)

## Impact

- **go.mod / go.sum**: Version constraints and checksums will change
- **crypto/crypto.go**: Refactored for modern error handling, potential use of `errors.Join`, `fmt.Errorf` with `%w`
- **webhooks/webhooks.go**: Same modernization; use `errors.New`/`%w` wrapping consistently
- **example/main.go**: Adopt `log/slog`, improve error handling, use `os.MkdirAll`, naming conventions (`has_attachments` → `hasAttachments`)
- **Downstream consumers**: Must use Go ≥ 1.26 to depend on this module
