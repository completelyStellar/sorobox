# sorobox — Agent Instructions

## Commands

| Action | Command |
|--------|---------|
| Build | `go build -o main.exe cmd/api/main.go` or `make build` |
| Run | `go run cmd/api/main.go` or `make run` |
| All tests | `go test ./... -v` or `make test` |
| Single pkg | `go test ./internal/server/... -v` |
| DB tests only | `go test ./internal/database -v` or `make itest` |
| Live reload | `make watch` (installs air if missing) |
| Docker DB up | `make docker-run` |
| Docker DB down | `make docker-down` |
| Clean binary | `make clean` |

## Architecture

- **Single Go module** (`sorobox`), Go 1.26, no external router (stdlib `net/http` + `ServeMux`)
- **Entrypoint**: `cmd/api/main.go` — graceful shutdown on SIGINT/SIGTERM
- **Database**: Postgres via `pgx/v5`, singleton connection (`internal/database/database.go`). Env-driven: `DB_DATABASE`, `DB_PASSWORD`, `DB_USERNAME`, `DB_PORT`, `DB_HOST`, `DB_SCHEMA`
- **Env loading**: `github.com/joho/godotenv/autoload` — auto-loads `.env`; no manual `Load()` call needed
- **Project vision**: Stellar/Soroban local dev sandbox with Go backend, React/TS/Tailwind frontend (via Tauri) — see `docs/PRD.md`

## Non-obvious gotchas

- `docker-compose.yml` env vars use `BLUEPRINT_DB_*` prefix but Go code reads `DB_*` — they won't connect without aligning. Copy `.env.example` to `.env` and adjust.
- `godotenv/autoload` imported in **both** `database.go` and `server.go` — redundant, loading happens on first import.
- `database.New()` is a **singleton** — returns the same `dbInstance` on subsequent calls.

## Scaffolding (planned, not yet created)

| Config | Expects | Status |
|--------|---------|--------|
| `sqlc.yaml` | `./migrations/`, `./internal/repository/query/` → `./internal/repository/db/` | Not created |
| CI integration test | `./integration/...` with `-tags=integration` | Not created |
| Frontend | `dashboard/` (React/TS/Tailwind per PRD) | Not created |

Run `sqlc generate` after creating migrations + query files.

## Lint & CI

- **Linter**: `golangci-lint` v2.12 (config: `.golangci.yml`), run via `golangci/golangci-lint-action`
- **CI**: lint → unit test → integration test (nightly or manual dispatch) → trivy security scan → goreleaser (on `v*` tag push)
- **Release**: goreleaser cross-compiles linux/darwin/windows amd64+arm64, binary name `sorobox`
- **Docker**: multi-stage build, `/bin/sorobox` binary

## Testing

- **Unit**: `internal/server/routes_test.go` — no DB needed, uses `httptest`
- **DB/integration**: `internal/database/database_test.go` — uses testcontainers to spin up Postgres (requires Docker)

## Style

- `gofumpt` formatting via golangci-lint
- CodeRabbit reviews Go files for idioms, error handling, race conditions, performance. Excludes `dashboard/**` and `docs/**`.
