## 1. Dependency Updates

- [x] 1.1 Bump `go` directive in `go.mod` from `1.17` to `1.26.0`
- [x] 1.2 Run `go get -u ./...` to update `golang.org/x/crypto` (and transitive deps) to latest semver
- [x] 1.3 Run `go mod tidy` to reconcile `go.sum` and remove stale entries
- [x] 1.4 Verify build compiles cleanly with `go build ./...`

## 2. Modernize crypto/crypto.go

- [x] 2.1 Handle discarded base64 decode errors — return descriptive errors instead of using `_`
- [x] 2.2 Wrap returned errors with `fmt.Errorf("...: %w", err)` in `Decrypt` and `DownloadAttachment`
- [x] 2.3 Verify the file compiles and behaviour is preserved

## 3. Modernize webhooks/webhooks.go

- [x] 3.1 Review error handling — ensure `fmt.Errorf` uses `%w` where wrapping an existing error
- [x] 3.2 Verify the file compiles and behaviour is preserved

## 4. Modernize example/main.go

- [x] 4.1 Rename `has_attachments` constant to `hasAttachments`
- [x] 4.2 Replace `log.Println` / `log.Panicln` with `log/slog` calls (`slog.Info`, `slog.Error`)
- [x] 4.3 Replace stat-then-mkdir pattern with `os.MkdirAll`
- [x] 4.4 Verify the example compiles cleanly

## 5. Final Verification

- [x] 5.1 Run `go vet ./...` — no warnings
- [x] 5.2 Run `go build ./...` — clean build
- [x] 5.3 Confirm `go mod tidy` produces no diff on a second run
