## ADDED Requirements

### Requirement: Errors use %w wrapping
All `fmt.Errorf` calls that wrap an existing error SHALL use the `%w` verb to enable `errors.Is` / `errors.As` by callers.

#### Scenario: Decrypt wraps JSON unmarshal error
- **WHEN** `crypto.Decrypt` fails during JSON unmarshalling
- **THEN** the returned error wraps the original unmarshal error via `%w`

#### Scenario: DownloadAttachment wraps HTTP and unmarshal errors
- **WHEN** `crypto.DownloadAttachment` fails during HTTP fetch or JSON unmarshalling
- **THEN** the returned error wraps the original error via `%w`

### Requirement: Base64 decode errors are not silently discarded
All `base64.StdEncoding.DecodeString` calls SHALL check and return the error instead of discarding it with `_`.

#### Scenario: Decrypt returns error on invalid base64 input
- **WHEN** `crypto.Decrypt` receives an `EncryptedBody` with malformed base64 in the encrypted content
- **THEN** the function returns an error describing the base64 decode failure

#### Scenario: DownloadAttachment returns error on invalid base64 attachment fields
- **WHEN** `crypto.DownloadAttachment` encounters malformed base64 in attachment nonce, public key, private key, or binary
- **THEN** the function returns an error describing which field failed to decode

### Requirement: Go naming conventions are followed
All local variables, constants, and unexported identifiers SHALL use camelCase per Go convention. Snake_case identifiers MUST be renamed.

#### Scenario: has_attachments renamed to hasAttachments
- **WHEN** a developer reads `example/main.go`
- **THEN** the constant formerly named `has_attachments` is named `hasAttachments`

### Requirement: Example uses structured logging
The example application SHALL use `log/slog` for all log output instead of `log.Println` / `log.Panicln`.

#### Scenario: Structured log output on startup
- **WHEN** the example server starts
- **THEN** startup messages are emitted via `slog.Info` (not `log.Println`)

#### Scenario: Structured error logging on failure
- **WHEN** a handler encounters an error
- **THEN** the error is logged via `slog.Error` with the error as a structured attribute

### Requirement: Directory creation uses MkdirAll
The example application SHALL use `os.MkdirAll` for creating the temp directory instead of a stat-then-mkdir pattern.

#### Scenario: Temp directory creation is idempotent
- **WHEN** the example server starts and the `temp` directory already exists
- **THEN** `os.MkdirAll` succeeds without error
