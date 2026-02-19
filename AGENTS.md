# Repository Guidelines

## Project Structure & Module Organization
- CLI entrypoint lives in `cmd/wacli` (Cobra commands and wiring).
- Core implementation lives in `internal/`:
  - `internal/app`: bootstrap/sync orchestration.
  - `internal/wa`: WhatsApp client integration.
  - `internal/store`: SQLite schema, queries, migrations, and search.
  - `internal/lock`, `internal/config`, `internal/pathutil`, `internal/out`: shared infrastructure.
- Design and release notes are in `docs/` (`docs/spec.md`, `docs/release.md`).
- Build output is generated under `dist/`.

## Build, Test, and Development Commands
- `pnpm -s build`: builds `dist/wacli` with `sqlite_fts5` tag.
- `pnpm -s start`: build then run CLI locally.
- `pnpm -s wacli -- --help`: run built CLI through the project wrapper.
- `pnpm -s test`: runs both standard and FTS-tagged test suites.
- `pnpm -s test:go`: run `go test ./...`.
- `pnpm -s test:fts`: run `go test -tags sqlite_fts5 ./...`.
- `pnpm -s lint`: run `go vet ./...`.
- `pnpm -s format:check`: fail if `gofmt` changes are needed.

## Coding Style & Naming Conventions
- Follow idiomatic Go formatting with `gofmt` (tabs, standard imports, no manual alignment).
- Keep packages focused by domain (`store`, `wa`, `app`, etc.) and files named by capability (for example `messages.go`, `media.go`).
- Tests use Go’s standard naming: files end with `_test.go`, functions use `TestXxx`.
- Prefer explicit, command-oriented naming in CLI code (for example `groups_refresh_list.go`).

## Testing Guidelines
- Add/adjust tests for every behavior change in the same package as the code.
- Run both test modes locally before opening a PR: `pnpm -s test`.
- Ensure FTS-related changes are validated with `sqlite_fts5` enabled (`pnpm -s test:fts`).

## Commit & Pull Request Guidelines
- Follow Conventional Commit style seen in history: `fix: ...`, `docs: ...`, `refactor: ...`, `ci: ...`, `chore: ...`.
- Keep subject lines imperative and scoped to one change.
- PRs should include:
  - What changed and why.
  - Linked issue(s) when applicable.
  - CLI output/examples for user-visible command changes.
- Before requesting review, ensure CI-equivalent checks pass: `format:check`, `lint`, `test`, and `build`.

## Security & Configuration Tips
- Do not commit local store/session data (`~/.wacli`), credentials, or environment secrets.
- Prefer environment overrides (`WACLI_DEVICE_LABEL`, `WACLI_DEVICE_PLATFORM`) instead of hardcoding device-specific values.
