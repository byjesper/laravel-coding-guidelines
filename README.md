# Laravel Coding Guidelines

Single source of truth for `byjesper`'s shared AI coding guidelines, plus a
zero-dependency tool that **publishes** them into a consuming repo's
`.ai/guidelines/` and (for non-Boost repos) **composes** `CLAUDE.md` / `AGENTS.md`.

The shared fragments live here once. Each project pulls them in with one composer
command — no copy/paste, and a `composer update` refreshes them everywhere.

## How it works

Two commands (`vendor/bin/byjesper-guidelines`, wrapped as composer scripts):

- **`sync --profile=apps|packages`** — copies this package's `common` +
  `<profile>` fragments into the consumer's **`.ai/guidelines/shared/`** (a
  *managed* directory — wiped and rewritten each run). Your own project-specific
  fragments live elsewhere under `.ai/guidelines/` and are **never touched**.
- **`build`** — composes every `.ai/guidelines/**/*.md` (shared + local), sorted
  by filename, into `CLAUDE.md` and `AGENTS.md`. For repos that do **not** use
  Laravel Boost.

Composition after `sync` depends on the repo:

| Repo type | After `sync` | Why |
| --------- | ------------ | --- |
| **App** (uses Boost) | `php artisan boost:update` | Boost reads `.ai/guidelines/**` and composes |
| **Library** (no Boost) | `composer guidelines:build` | small built-in composer; keeps Boost out of libraries |

`CLAUDE.md` / `AGENTS.md` and `.ai/guidelines/shared/` are **generated** — never
edit them. Edit the fragments here (shared) or your local `.ai/guidelines/*.md`,
then re-sync / re-build.

## Use it in a library (no Boost)

```jsonc
// composer.json — once this repo is public on Packagist
"require-dev": { "byjesper/laravel-coding-guidelines": "^0.1" },
"scripts": {
    "post-update-cmd": ["@guidelines:sync", "@guidelines:build"],
    "guidelines:sync": "byjesper-guidelines sync --profile=packages",
    "guidelines:build": "byjesper-guidelines build",
    "guidelines:check": "byjesper-guidelines build --check"
}
```

Wire `@guidelines:check` into your `test` script so CI fails on hand-edited
`CLAUDE.md`/`AGENTS.md`. `post-update-cmd` auto-refreshes on every
`composer update`.

## Use it in an app (Boost)

```jsonc
"require-dev": { "byjesper/laravel-coding-guidelines": "^0.1" },
"scripts": {
    "post-update-cmd": ["@guidelines:sync"],
    "guidelines:sync": "byjesper-guidelines sync --profile=apps"
}
```

Then `php artisan boost:update` composes `.ai/guidelines/**` (shared + local)
into `CLAUDE.md`/`AGENTS.md`.

## Where project-specific guidance goes

Anything unique to a project — most importantly its **git release-model**
declaration — is a local fragment in `.ai/guidelines/` (e.g.
`55-git-release-model.md`). `sync` only manages `.ai/guidelines/shared/`, so your
local fragments are safe and compose alongside the shared ones (ordered by
filename number).

## Profiles

| Profile    | Gets              | For                  |
| ---------- | ----------------- | -------------------- |
| `packages` | common + packages | composer libraries   |
| `apps`     | common + apps     | Laravel applications |

Add a profile by creating a new directory under `guidelines/`; `sync` discovers
it automatically.

## License

The MIT License (MIT). See [LICENSE.md](LICENSE.md).
