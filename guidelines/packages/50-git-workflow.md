# Git Workflow (Packages)

- `main` must always be releasable.
- Use a branch for anything beyond trivial docs/metadata: feature work,
  behaviour changes, migrations, public API or query-behaviour changes.
- Branch name prefixes: `feat/`, `fix/`, `docs/`, `ci/`.
- Prefer issue-backed work — capture intent before larger changes begin.
- Once a package is public, protect `main` and require pull requests for
  external contributions.
- External pull requests must pass CI and include tests for behaviour changes.
