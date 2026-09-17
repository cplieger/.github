# Contributing

Thanks for contributing. These defaults apply across `cplieger` repos; a repo
may override them with its own `CONTRIBUTING.md`.

By participating you agree to abide by our
[Code of Conduct](CODE_OF_CONDUCT.md). Report security vulnerabilities through
the [security policy](SECURITY.md), never in a public issue. For usage
questions, see [SUPPORT.md](SUPPORT.md).

## Workflow

1. Fork / branch from `main`.
2. Make focused changes with tests.
3. Ensure CI passes locally where possible (lint, typecheck/vet, tests).
4. Open a PR; fill in the template.

## Commit messages

In the repos the central pipeline releases, releases are automated from
**[Conventional Commits](https://www.conventionalcommits.org/)** via git-cliff;
your commit type determines the version bump:

| Prefix | Effect |
| --- | --- |
| `feat:` | new feature |
| `fix:` | bug fix |
| `sec:` | security fix |
| `refactor:` `perf:` | patch release, listed under Changed |
| `chore:` `ci:` `docs:` `style:` `test:` | no release |
| `feat!:` / `BREAKING CHANGE:` | breaking change |

Pre-1.0 stays within `0.x` (see the repo's `cliff.toml`, where one exists).

## Code style

Formatting and linting are enforced in CI (golangci-lint for Go; eslint and
prettier for TypeScript; ruff for Python; shellcheck, shfmt, and hadolint for
shell and Dockerfiles; markdownlint for every repo). Run the relevant tool
before pushing. [`cplieger/ci`](https://github.com/cplieger/ci) carries
`ci-local.sh`, which replays a repo's whole CI battery locally.

## Code review

All changes reach `main` through a pull request and must pass the required
`ci / validate` status check (lint, typecheck/vet, tests, and gitleaks secret
scanning) before they can merge. CodeQL runs on every pull request, and Trivy
runs on pull requests that touch dependencies, Dockerfiles, or shell scripts;
both report to the Security tab. Their findings are triaged and resolved as
part of review rather than gating the merge mechanically. The maintainer
reviews every pull request a person opens, external ones included, before
merging; automated dependency and config-sync pull requests merge on green CI.

Review explicitly covers the security impact of a change, not just
correctness: new or changed handling of untrusted input, trust boundaries,
dependency and supply-chain changes, secret handling, and anything the
automated scanners flag. Findings are resolved before merge, not after.

This is currently a single-maintainer project; see
[GOVERNANCE.md](GOVERNANCE.md) and [CONTINUITY.md](CONTINUITY.md) for the
governance and continuity plan.
