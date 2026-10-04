# Contributing

These rules apply to every public cplieger repository.

## Commits and releases

Commits follow [Conventional Commits](https://www.conventionalcommits.org/). The subject of each releasing commit becomes a line in the release notes, so write it for the person who reads them.

In a repository with a `cliff.toml`, the commit type decides whether a change releases and its version number:

| Commit | Release |
| --- | --- |
| `feat:` | minor version, listed under Added |
| `fix:` | patch version, listed under Fixed |
| `sec:` | patch version, listed under Security |
| `refactor:`, `perf:` or a type this table does not name | patch version, listed under Changed |
| `chore(deps):` | patch version, listed under Dependencies |
| `chore:`, `chore(devdeps):`, `ci:`, `docs:`, `style:`, `test:`, `fuzz:`, `lint:`, `debug:`, `release:` | no release |
| `!` after a type that releases, or a `BREAKING CHANGE:` footer on one | major version |

A hardening fix that adds no public API is a `sec:` commit, not a `feat:`.

A repository with no release yet starts at `v1.0.0`. A repository whose latest version is below 1.0 stays in `0.x`. There a `feat:` commit raises the patch version and a breaking change raises the minor version.

The release notes print a `BREAKING CHANGE:` footer as the upgrade steps. Write it as a bullet list, one step per bullet.

A commit that changes only these files never releases, whatever its type:

- Markdown files, the root `LICENSE` and the root `.github/` folder
- the root `alerts/` folder and the root `compose.yaml`
- the root `tests/` folder, any `testdata/` folder, and `_test.go`, `.test.ts` and `.spec.ts` files
- any `package-lock.json`, `.punused-ignore` or knip configuration file
- the root `.editorconfig`, `.gitattributes`, `.gitignore` and `.dockerignore`

## Files that belong to cplieger/ci

A file whose opening comment says it is synced from [cplieger/ci](https://github.com/cplieger/ci) is a copy that the next sync replaces. Change it there. `.prettierrc.json`, `.stylelintrc.json` and `.htmlvalidate.json` are copies too, because a JSON file cannot carry that line.

## Checks

Clone [cplieger/ci](https://github.com/cplieger/ci) next to the repository. From the repository root, run `bash ../ci/ci-local.sh` before you push. A result that reports `check(s) not validated locally` leaves those checks to CI.

The `scripts/install-local-tools.sh` script in cplieger/ci installs the tool versions CI uses.

In a package published to npm or JSR, the `version` field in `package.json` and `jsr.json` is a placeholder that the release replaces with the tag's version. Leave it as it is.

## Review

The maintainer reviews every pull request a person opens before it merges. A library takes a new option or exported name when a cplieger app benefits from it now or soon, or when the concept the library models expects it.

If an AI assistant wrote a change, have other AI agents review it in depth before you open the pull request. Ideally they run a model from a different provider than the one that wrote it. Use a top-tier model such as Claude Opus for that review, not a small fast one. Open the pull request from the reviewed version, never from the first draft.

## Conduct and security

Everyone taking part follows the [Code of Conduct](CODE_OF_CONDUCT.md). Report a vulnerability privately through the [security policy](SECURITY.md), never in a public issue. [SUPPORT.md](SUPPORT.md) says where to ask a question.
