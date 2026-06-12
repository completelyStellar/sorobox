# Contributing to Sorobox

Thanks for your interest! Here's how to contribute, whether you're fixing a bug, adding a feature, or improving docs.

## Table of Contents

- [Branch Strategy](#branch-strategy)
- [Reporting Issues](#reporting-issues)
- [Development Setup](#development-setup)
- [Making Changes](#making-changes)
- [Commit Guidelines](#commit-guidelines)
- [Pull Request Process](#pull-request-process)
- [Code Style](#code-style)
- [Testing](#testing)

---

## Branch Strategy

```
feature/my-feature  →  deploy  →  main (release only)
```

- **`deploy`** — the default branch. All pull requests target this branch.
- **`main`** — release-ready code only. A maintainer merges `deploy` into `main` when cutting a release. Never open a PR against `main` directly.

Feature branches should be created off `deploy` and named descriptively:

| Branch name | Purpose |
|---|---|
| `feat/account-export` | New feature |
| `fix/db-timeout` | Bug fix |
| `docs/api-readme` | Documentation |
| `chore/update-deps` | Maintenance |

---

## Reporting Issues

Before opening an issue, search [existing issues](https://github.com/completelyStellar/sorobox/issues) to avoid duplicates.

When opening a bug report, include:

- **Go version** (`go version`)
- **OS version** (Windows/macOS/Linux)
- **What you were doing** when the bug occurred
- **Expected vs actual behavior**
- **Logs or error output** (if any)
- **Steps to reproduce** — be specific

Feature requests should explain the use case and why it matters, not just what you want built.

---

## Development Setup

### Prerequisites

- [Go](https://go.dev/dl/) 1.26 or later
- [Docker Desktop](https://www.docker.com/products/docker-desktop/) (for the database and integration tests)
- [Make](https://www.gnu.org/software/make/) (bundled on macOS/Linux; on Windows via [Chocolatey](https://community.chocolatey.org/packages/make) or [Git Bash](https://git-scm.com/))

### Step-by-step

```bash
# 1. Clone the repository
git clone https://github.com/completelyStellar/sorobox.git
cd sorobox

# 2. Create your environment file from the example
cp .env.example .env

# 3. Start the PostgreSQL database (Docker)
make docker-run

# 4. Run the API server (leave this running)
make run
```

The API will be available at `http://localhost:8080`. Verify with:

```bash
curl http://localhost:8080/health
```

### Environment Variables

The app reads database settings from these environment variables (defined in `.env`):

| Variable | Default | Description |
|---|---|---|
| `PORT` | `8080` | HTTP server port |
| `APP_ENV` | `local` | Runtime environment |
| `DB_HOST` | `localhost` | Database host |
| `DB_PORT` | `5432` | Database port |
| `DB_DATABASE` | `sorobox` | Database name |
| `DB_USERNAME` | `postgres` | Database user |
| `DB_PASSWORD` | `postgres` | Database password |
| `DB_SCHEMA` | `public` | Database schema |

**Note:** The `docker-compose.yml` uses a `BLUEPRINT_DB_*` prefix. If you adjust those values, make sure your `.env` `DB_*` values match.

### Troubleshooting

| Problem | Likely cause | Fix |
|---|---|---|
| `go: go.mod file not found` | Not in project root | `cd sorobox` |
| `connection refused` on DB | Docker not running | `make docker-run` |
| Port 5432 already in use | Another Postgres running | Stop it, or change `DB_PORT` in `.env` |
| `air: command not found` | `make watch` first run | It auto-installs, just re-run |

---

## Making Changes

```bash
# 1. Switch to the deploy branch and pull latest
git checkout deploy
git pull origin deploy

# 2. Create a feature branch
git checkout -b feat/my-feature

# 3. Make your changes
# 4. Run tests to verify nothing is broken
make test

# 5. Stage and commit
git add .
git commit -m "feat: add account export endpoint"

# 6. Push your branch
git push origin feat/my-feature
```

Then open a pull request against `deploy` on GitHub.

---

## Commit Guidelines

Write clear, atomic commits. Each commit should be one logical change.

```
type(scope): short description

Optional longer explanation of what and why.
```

Types:

| Type | When to use |
|---|---|
| `feat` | A new feature |
| `fix` | A bug fix |
| `docs` | Documentation only |
| `style` | Formatting, missing semicolons, etc. (no code change) |
| `refactor` | Code change that neither fixes a bug nor adds a feature |
| `test` | Adding or fixing tests |
| `chore` | Build process, dependencies, tooling |

Examples:

```
feat(accounts): add CSV export endpoint
fix(db): handle connection timeout on startup
docs: fix broken link in README
test(server): add health endpoint test
```

Keep the subject line under 72 characters.

---

## Pull Request Process

### Before you open a PR

1. [ ] Pull latest `deploy` and rebase your branch: `git rebase deploy`
2. [ ] Run `make test` — all tests pass
3. [ ] Run `golangci-lint run` — no new warnings (or `make lint` if available)
4. [ ] Check that your code follows the project's [code style](#code-style)
5. [ ] Write or update tests to cover your changes (aim for at least the happy path)
6. [ ] Update any affected documentation

### Opening the PR

- Title your PR using the same convention as commits: `type(scope): description`
- In the description, explain **what** changed and **why**
- If your PR fixes an issue, include `Closes #123` to auto-close it on merge
- Add a screenshot or terminal output if the change is visual or behavioral

### Review & Merge

- A maintainer will review your PR. They may request changes — that's normal!
- Once approved, your PR will be **squash-merged** into `deploy`
- After enough changes accumulate, a maintainer merges `deploy` into `main` and tags a release

---

## Code Style

- **Formatter**: We use [`gofumpt`](https://github.com/mvdan/gofumpt) (a stricter `gofmt`). Run `golangci-lint run` to check formatting.
- **Imports**: Standard library first, then third-party, then internal. Use `gofumpt` to sort.
- **Errors**: Always handle them. Don't use `_` to discard an error unless there's a strong reason and a comment explains it.
- **Comments**: Go-style `//` comments. Comments explain *why*, not *what* — the code should make the what obvious.
- **Names**: Short names for short-lived variables (`i`, `err`). Longer descriptive names for exported symbols.
- **Testing**: Table-driven tests preferred. Use `t.Run` for sub-tests. Use `httptest` for HTTP handlers.

The project's linter configuration is in `.golangci.yml` — running the same linter locally catches issues before CI.

---

## Testing

```bash
# Run all tests
make test

# Run only unit tests (no Docker needed)
go test ./internal/server/... -v

# Run database integration tests (requires Docker)
make itest
```

### Unit tests

Located in `internal/server/`. They test HTTP handlers using Go's `httptest` package. No database or external services needed.

### Integration tests

Located in `internal/database/`. They use [testcontainers-go](https://golang.testcontainers.org/) to spin up a real PostgreSQL instance inside Docker. These tests:

- Require Docker Desktop to be running
- Take ~10 seconds to start the container
- Clean up the container automatically after the test run

### Before submitting

Always `make test` before pushing. If CI fails, check the logs and fix the issue — don't dismiss CI failures.

---

## Questions?

Open a [discussion](https://github.com/completelyStellar/sorobox/discussions) or ask in the PR itself. We're happy to help.
