# Git & Release Model (Apps)

- **The branching and release model is project-specific.** Each app declares it
  at the top of its own `.ai/guidelines` Git section (e.g. a single
  `develop`-integration line, vs. a maintenance-branch model with `n.n.x`
  release lines). Do **not** assume one model carries across apps — re-read the
  project's declaration before you branch, merge, or advise a release line.
- Universal git rules (branch-name prefixes, no `git -C`, full-suite-before-
  merge, no auto-merge, the pure-docs exception) live in [[50-git-hygiene]].
- **Deploy only tagged releases** (e.g. `v6.0.5`) — never a branch directly.
- Keep migrations forward-only and reversible where practical.
