## ADDED Requirements

### Requirement: Go version targets 1.26.0
The `go` directive in `go.mod` SHALL be set to `1.26.0`.

#### Scenario: go.mod reflects Go 1.26.0
- **WHEN** a developer inspects `go.mod`
- **THEN** the `go` directive reads `go 1.26.0`

### Requirement: Direct dependencies use latest semver releases
All direct dependencies in `go.mod` SHALL reference the latest stable tagged version (not pseudo-versions).

#### Scenario: x/crypto is at latest tagged version
- **WHEN** `go list -m golang.org/x/crypto` is run after update
- **THEN** the version is a semver tag (e.g. `v0.X.Y`), not a `v0.0.0-YYYYMMDDhhmmss-hash` pseudo-version

### Requirement: Indirect dependencies are reconciled
Running `go mod tidy` SHALL produce a clean `go.sum` with no extraneous entries.

#### Scenario: go mod tidy is clean
- **WHEN** `go mod tidy` is run after all dependency updates
- **THEN** the command exits with code 0 and produces no diff on a second run
