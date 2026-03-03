## Context

The FormSG Golang SDK provides two packages — `crypto` (NaCl box decryption of form submissions) and `webhooks` (Ed25519 signature verification). The module currently targets Go 1.17 with a 2022-era `golang.org/x/crypto` snapshot. All source files use pre-1.18 idioms. The codebase is small (~370 LOC across three files) making a one-shot modernization practical.

## Goals / Non-Goals

**Goals:**
- Update `go.mod` to Go 1.26.0 and all dependencies to latest semver releases
- Modernize all Go source to idiomatic Go 1.22+ style
- Maintain identical runtime behaviour — no functional changes

**Non-Goals:**
- Adding new SDK features or API surface
- Changing the public API signatures (types, function names, package structure)
- Adding unit tests (separate effort)
- Adopting a different crypto library

## Decisions

### D1: Bump go directive to 1.26.0 directly (skip intermediate versions)

**Rationale:** The SDK has no known downstream consumers pinned to older Go. A single jump is simpler than staged bumps. 1.26 is the current stable release.

**Alternative considered:** Incremental bumps (1.17 → 1.21 → 1.26). Rejected — adds complexity with no benefit for a library this size.

### D2: Use `go get -u ./... && go mod tidy` for dependency updates

**Rationale:** Standard Go toolchain workflow. With only one direct dependency (`x/crypto`), manual pinning is unnecessary.

**Alternative considered:** Manually editing `go.mod` version strings. Rejected — error-prone and doesn't update `go.sum`.

### D3: Modernize error handling with `fmt.Errorf` `%w` wrapping

**Rationale:** All errors in the codebase use `fmt.Errorf("message")` without wrapping. Using `%w` enables callers to use `errors.Is` / `errors.As` for programmatic error inspection.

**Alternative considered:** Define sentinel errors. Deferred — overkill for the current API surface; `%w` wrapping is sufficient as a first step.

### D4: Rename `has_attachments` to `hasAttachments` in example

**Rationale:** Go convention is camelCase for local variables and constants. The underscore style is a leftover from another language's conventions.

### D5: Use `log/slog` for structured logging in example

**Rationale:** Available since Go 1.21, `slog` is the stdlib structured logger. The example currently uses `log.Println` / `log.Panicln` inconsistently. `slog` gives consistent structured output.

**Alternative considered:** Keep `log` package. Acceptable but misses the modernization goal.

### D6: Replace `os.Mkdir` with `os.MkdirAll` in example

**Rationale:** `MkdirAll` is idempotent and handles nested paths. Simplifies the stat-then-mkdir pattern currently used.

## Risks / Trade-offs

- **[Breaking downstream consumers]** → Mitigated by the fact that no known consumers are pinned to Go < 1.26. The module path stays the same.
- **[Silently changed error strings]** → Wrapping with `%w` changes `err.Error()` output slightly. No programmatic impact since current errors aren't sentinel types, but log output may differ.
- **[x/crypto API changes]** → `nacl/box` API is frozen. Risk is negligible.
