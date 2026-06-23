# Laravel Coding Guidelines

Shared AI coding guidelines for `byjesper` Laravel packages and apps, plus a
zero-dependency generator that compiles them into `CLAUDE.md` and `AGENTS.md`.

`CLAUDE.md` and `AGENTS.md` are **generated artifacts** — never edit them by
hand. Edit the fragments instead and rebuild.

## How it works

Fragments are concatenated in order:

1. `guidelines/common/*.md` — shared by every profile
2. `guidelines/<profile>/*.md` — `packages` or `apps`
3. `./.ai/guidelines/*.md` — local to the consuming repo

Files sort by name within each group, so number them (`10-`, `20-`, …).

## Use it in a package or app

Once this repo is public and registered on [Packagist](https://packagist.org),
consumers need nothing but the dev requirement — no `repositories` block:

```jsonc
// composer.json
"require-dev": {
    "byjesper/laravel-coding-guidelines": "^1.0"
},
"scripts": {
    "guidelines:build": "byjesper-guidelines --profile=packages",
    "guidelines:check": "byjesper-guidelines --profile=packages --check"
}
```

> If the repo is kept private/unpublished instead, each consumer must add a
> `"repositories": [{ "type": "vcs", "url": "…" }]` entry pointing at it.
> Publishing publicly to Packagist avoids that per-consumer wiring.

Apps use `--profile=apps` instead. Wire `@guidelines:check` into the `test`
script so CI fails if `CLAUDE.md`/`AGENTS.md` drift from source.

```bash
composer guidelines:build   # regenerate CLAUDE.md + AGENTS.md
composer guidelines:check   # verify they are up to date (CI)
```

## Profiles

| Profile    | Gets            | For                          |
| ---------- | --------------- | ---------------------------- |
| `packages` | common + packages | composer libraries         |
| `apps`     | common + apps     | Laravel applications       |

Add a profile by creating a new directory under `guidelines/`; the generator
discovers it automatically.

## License

The MIT License (MIT). See [LICENSE.md](LICENSE.md).
