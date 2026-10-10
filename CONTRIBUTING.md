# Contributing

These rules cover every public cplieger repository.

## Commits

Each pull request is squash-merged under its title. Write it in [Conventional Commits](https://www.conventionalcommits.org/) form, as the change users see: `fix: subtitles in mov_text tracks decode again`.

A change users notice gets a one-to-three-sentence `## Release note` in the description. Leave the `deps` and `devdeps` scopes to Renovate.

A hardening fix with no new public API is `sec:`.

A breaking change carries `!` in the title and ends the description with a `BREAKING CHANGE:` footer, one bullet per upgrade step.

## Repositories whose default branch is `dev`

Your pull request goes to `dev`, where a releasing merge publishes a pre-release such as `v1.4.0-dev.2`.

The files a merge changes decide whether it releases. Its title becomes a line in the release-notes section its commit type picks:

| Commit | Section |
| --- | --- |
| `sec:` | Security |
| `feat:` | Added |
| `fix:` | Fixed |
| `perf:` | Performance |
| `refactor:` or an unlisted type | Changed |
| `chore:`, `chore(deps):`, `chore(devdeps):`, `fix(deps):`, `ci:`, `docs:`, `style:`, `test:`, `fuzz:`, `lint:`, `debug:`, `release:` | no line |

Changes only to these files never release:

- Markdown files, PNG, JPEG, WebP, GIF and SVG files in the root `docs/` folder, the root `LICENSE` and the root `.github/` folder
- the root `alerts/` folder and the root `compose.yaml`
- the root `tests/` folder, any `testdata/` folder, and `_test.go`, `.test.ts` and `.spec.ts` files
- any `package-lock.json` or knip configuration file
- the root `.editorconfig`, `.gitattributes`, `.gitignore` and `.dockerignore`

`main` holds the released version and takes only promotions of `dev` and automated pull requests. An automated pull request that changes shipped files releases the next patch.

A [promotion](https://github.com/cplieger/ci/blob/main/docs/workflows.md#the-two-branch-release-model) releases the next minor, or from 1.0 the next major when it carries a breaking change. Below 1.0 a breaking change raises the minor. An unreleased repository starts at `v1.0.0`.

## Synced files

The next sync replaces a file whose opening comment says it is synced from [cplieger/ci](https://github.com/cplieger/ci), and `.prettierrc.json`, `.stylelintrc.json` and `.htmlvalidate.json`. Change them there.

## Checks

Before pushing, run `bash ../ci/ci-local.sh` from the repository root with [cplieger/ci](https://github.com/cplieger/ci) cloned beside it. Its `scripts/install-local-tools.sh` installs CI's tool versions. A `check(s) not validated locally` line leaves those checks to CI.

Leave the placeholder `version` in `package.json` and `jsr.json` to the release.

## Review

The maintainer reviews every pull request a person opens before it merges. A library takes a new option or exported name when a cplieger app benefits from it now or soon, or when the concept the library models expects it.

Open an issue first to add a dependency, test-only ones included, to add what the README lists as unsupported by design, or to weaken a guarantee the README states.

If an AI assistant wrote a change, have other AI agents review it in depth before you open the pull request. Ideally they run a model from a different provider than the one that wrote it. Use a top-tier model such as Claude Opus for that review, not a small fast one. Open the pull request from the reviewed version, never from the first draft.

## Conduct and security

The [Code of Conduct](CODE_OF_CONDUCT.md) applies to everyone. Report a vulnerability privately through the [security policy](SECURITY.md). [SUPPORT.md](SUPPORT.md) says where to ask.
