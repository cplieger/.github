# Contributing

These rules apply to every public cplieger repository.

## Commits and releases

Each pull request is squash-merged with its title as the release-notes line. Write it in [Conventional Commits](https://www.conventionalcommits.org/) form, as the change users see: `fix: subtitles in mov_text tracks decode again`. With one commit, make the title its subject.

A change users notice gets a one-to-three-sentence `## Release note` in the description. Leave the `deps` and `devdeps` scopes to Renovate.

With a `cliff.toml` and default branch `main`, the commit type decides the release:

| Commit | Version | Section |
| --- | --- | --- |
| `feat:` | minor | Added |
| `fix:` | patch | Fixed |
| `sec:` | patch | Security |
| `refactor:`, `perf:` or an unlisted type | patch | Changed |
| `chore(deps):` | patch | Dependencies |
| `chore:`, `chore(devdeps):`, `ci:`, `docs:`, `style:`, `test:`, `fuzz:`, `lint:`, `debug:`, `release:` | no release | |
| a releasing type with `!` or a `BREAKING CHANGE:` footer | major | |

A hardening fix with no new public API is `sec:`. A repository with no release starts at `v1.0.0`. Below 1.0, `feat:` raises the patch version and a breaking change the minor.

A breaking change carries `!` in the title and ends the description with a `BREAKING CHANGE:` footer, one bullet per upgrade step.

Changes only to these files never release:

- Markdown files, the root `LICENSE` and the root `.github/` folder
- the root `alerts/` folder and the root `compose.yaml`
- the root `tests/` folder, any `testdata/` folder, and `_test.go`, `.test.ts` and `.spec.ts` files
- any `package-lock.json`, `.punused-ignore` or knip configuration file
- the root `.editorconfig`, `.gitattributes`, `.gitignore` and `.dockerignore`

## Repositories whose default branch is `dev`

Your pull request goes to `dev`. `main` holds the released version and takes only promotions of `dev` and automated pull requests, which release a patch when they change shipped files.

A [promotion](https://github.com/cplieger/ci/blob/main/docs/workflows.md#the-two-branch-release-model) releases the next minor version, or from 1.0 the next major for a breaking change.

## Synced files

The next sync replaces a file whose opening comment says it is synced from [cplieger/ci](https://github.com/cplieger/ci), and `.prettierrc.json`, `.stylelintrc.json` and `.htmlvalidate.json`. Change them there.

## Checks

Before you push, run `bash ../ci/ci-local.sh` from the repository root with [cplieger/ci](https://github.com/cplieger/ci) cloned beside it. Its `scripts/install-local-tools.sh` installs CI's tool versions. A `check(s) not validated locally` result leaves them to CI.

Leave the placeholder `version` in `package.json` and `jsr.json` to the release.

## Review

The maintainer reviews every pull request a person opens before it merges. A library takes a new option or exported name when a cplieger app benefits from it now or soon, or when the concept the library models expects it.

Open an issue first to add a dependency, test-only ones included, to add what the README lists as unsupported by design, or to weaken a guarantee the README states.

If an AI assistant wrote a change, have other AI agents review it in depth before you open the pull request. Ideally they run a model from a different provider than the one that wrote it. Use a top-tier model such as Claude Opus for that review, not a small fast one. Open the pull request from the reviewed version, never from the first draft.

## Conduct and security

The [Code of Conduct](CODE_OF_CONDUCT.md) applies to everyone. Report a vulnerability privately through the [security policy](SECURITY.md). [SUPPORT.md](SUPPORT.md) says where to ask.
