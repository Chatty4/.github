# .github
Shared PR and issue templates and reusable CI workflows for all Chatty repos.

## Reusable workflows

### `python-ci.yml`
Lint (`ruff check`, `ruff format --check`) and tests (`pytest`) for the Python services.

```yaml
# <repo>/.github/workflows/ci.yml
name: CI
on:
  pull_request:
  push:
    branches: [main, dev]

jobs:
  ci:
    uses: Chatty4/.github/.github/workflows/python-ci.yml@dev
    with:
      needs-postgres: true      # starts postgres:17-alpine and exports DATABASE_URL
      test-env: |
        SECRET_KEY=ci-not-secret
```

| Input | Default | What it does |
|---|---|---|
| `python-version` | `3.14` | Python for setup-python |
| `working-directory` | `.` | Folder with `requirements.txt` and `pyproject.toml` |
| `run-tests` | `true` | Run pytest. Set `false` while a repo has no tests |
| `needs-postgres` | `false` | Start Postgres 17 and export `DATABASE_URL` (on `127.0.0.1`) |
| `postgres-db` / `-user` / `-password` | `test_db` / `test_user` / `test_pass` | Credentials for that throwaway database |
| `test-env` | empty | Extra `KEY=VALUE` lines exported before pytest. Not for real secrets |

Lint installs only the `ruff==` pin from `requirements.txt`, so CI uses the same ruff version as local runs.
