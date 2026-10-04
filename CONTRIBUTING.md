# Contributing

These rules apply to every public cplieger repository.

## Commits and releases

Commits follow [Conventional Commits](https://www.conventionalcommits.org/). The subject of each releasing commit becomes a line in the release notes, so write it for that reader.

With a `cliff.toml`, the commit type decides whether a change releases and its version number:

| Commit | Release |
| --- | --- |
| `feat:` | minor version, listed under Added |
| `fix:` | patch version, listed under Fixed |
| `sec:` | patch version, listed under Security |
| `refactor:`, `perf:` or a type this table does not name | patch version, listed under Changed |
| `chore(deps):` | patch version, listed under Dependencies |
| `chore:`, `chore(devdeps):`, `ci:`, `docs:`, `style:`, `test:`, `fuzz:`, `lint:`, `debug:`, `release:` | no release |
| `!` after a type that releases, or a `BREAKING CHANGE:` footer on one | major version |

A hardening fix that adds no public API is `sec:`, not `feat:`.

A repository with no release starts at `v1.0.0`. One whose latest version is below 1.0 stays in `0.x`, where a `feat:` commit raises the patch version and a breaking change raises the minor version.

The release notes print a `BREAKING CHANGE:` footer as the upgrade steps, so write it as one bullet per step.

A commit that changes only these files never releases, whatever its type:

- Markdown files, the root `LICENSE` and the root `.github/` folder
- the root `alerts/` folder and the root `compose.yaml`
- the root `tests/` folder, any `testdata/` folder, and `_test.go`, `.test.ts` and `.spec.ts` files
- any `package-lock.json`, `.punused-ignore` or knip configuration file
- the root `.editorconfig`, `.gitattributes`, `.gitignore` and `.dockerignore`

## Files that belong to cplieger/ci

A file whose opening comment says it is synced from [cplieger/ci](https://github.com/cplieger/ci) is a copy that the next sync replaces. Change it there. `.prettierrc.json`, `.stylelintrc.json` and `.htmlvalidate.json` are copies too, because a JSON file cannot carry that line.

## Checks

Clone [cplieger/ci](https://github.com/cplieger/ci) next to the repository and run `bash ../ci/ci-local.sh` from the repository root before you push. A `check(s) not validated locally` result leaves those checks to CI.

`scripts/install-local-tools.sh` in cplieger/ci installs the tool versions CI uses.

In an npm or JSR package, the `version` field in `package.json` and `jsr.json` is a placeholder the release replaces. Leave it.

## Review

The maintainer reviews every pull request a person opens before it merges. A library takes a new option or exported name when a cplieger app benefits from it now or soon, or when the concept the library models expects it.

Open an issue first to add a dependency, test-only ones included, to add what the README lists as unsupported by design, or to weaken a guarantee the README states.

If an AI assistant wrote a change, have other AI agents review it in depth before you open the pull request. Ideally they run a model from a different provider than the one that wrote it. Use a top-tier model such as Claude Opus for that review, not a small fast one. Open the pull request from the reviewed version, never from the first draft.

## Conduct and security

Everyone taking part follows the [Code of Conduct](CODE_OF_CONDUCT.md). Report a vulnerability privately through the [security policy](SECURITY.md), never in a public issue. [SUPPORT.md](SUPPORT.md) says where to ask.
