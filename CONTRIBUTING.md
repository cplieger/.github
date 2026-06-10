# Contributing

Thanks for contributing. These defaults apply across `cplieger` repos; a repo
may override them with its own `CONTRIBUTING.md`.

By participating you agree to abide by our
[Code of Conduct](CODE_OF_CONDUCT.md). Report security vulnerabilities through
the [security policy](SECURITY.md) — never in a public issue. For usage
questions, see [SUPPORT.md](SUPPORT.md).

## Workflow

1. Fork / branch from `main`.
2. Make focused changes with tests.
3. Ensure CI passes locally where possible (lint, typecheck/vet, tests).
4. Open a PR; fill in the template.

## Commit messages

Releases are automated from **[Conventional Commits](https://www.conventionalcommits.org/)**
via git-cliff — your commit type determines the version bump:

| Prefix | Effect |
|---|---|
| `feat:` | new feature |
| `fix:` | bug fix |
| `sec:` | security fix |
| `chore:` `ci:` `docs:` `style:` `refactor:` `perf:` `test:` | no release |
| `feat!:` / `BREAKING CHANGE:` | breaking change |

Pre-1.0 stays within `0.x` (see each repo's `cliff.toml`).

## Code style

Formatting and linting are enforced in CI (golangci-lint for Go; eslint +
prettier for TypeScript). Run the repo's lint/format targets before pushing.
