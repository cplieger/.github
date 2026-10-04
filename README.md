# .github

This repository holds the default community-health files and the shared Renovate preset for the cplieger GitHub account. It is licensed under Apache-2.0. You can copy any file into your own `.github` repository, but the links, labels and policies in them are written for cplieger repositories.

## What GitHub uses from here

GitHub uses a file from this repository for any cplieger repository, public or private, that has no file of that type itself. A repository's own copy in `.github/`, its root or `docs/` takes precedence. A repository with its own `.github/ISSUE_TEMPLATE` folder gets none of the default forms. GitHub shows the defaults on its website only, and they are not part of a repository's clones, packages or downloads. The defaults work only while this repository is public.

- `CODE_OF_CONDUCT.md` is adapted from the [Contributor Covenant 3.0](https://www.contributor-covenant.org/version/3/0/code_of_conduct/), which is licensed under CC BY-SA 4.0.
- `CONTRIBUTING.md` covers the commit types that decide each release, the files that never release, the files synced from cplieger/ci, the local checks and review.
- `SECURITY.md` explains how to report a vulnerability privately from a repository's Security tab and how to verify a release. The maintainer acknowledges a report within 7 days and releases a fix before public disclosure, normally within 90 days of the report.
- `SUPPORT.md` says where to ask a question, report a bug or report a vulnerability.
- `.github/ISSUE_TEMPLATE/` holds the bug report, feature request and question forms. Its `config.yml` turns off blank issues and links the security policy.
- `.github/PULL_REQUEST_TEMPLATE.md` asks for a summary, linked issues, the changes, testing and a checklist.
- `.github/FUNDING.yml` is a placeholder with every entry commented out, so no Sponsor button appears.

The forms add the `bug`, `enhancement` and `question` labels. GitHub needs a form's label to exist in the repository that uses the form. All three are among the labels GitHub creates in every new repository.

## Account-wide documents GitHub does not serve

`GOVERNANCE.md`, `ROADMAP.md` and `CONTINUITY.md` cover the cplieger repositories as a whole, but GitHub does not use them as defaults. All three are in this repository.

- `GOVERNANCE.md` describes the single-maintainer model, how decisions are made and how a contributor can become a maintainer.
- `ROADMAP.md` is the default roadmap for the mature projects in maintenance mode. A repository that is still evolving may keep its own `ROADMAP.md`, which takes precedence. No repository keeps one today.
- `CONTINUITY.md` is the continuity and access-recovery plan. It states the single-maintainer risk and the steps planned to reduce it.

## What each repository keeps itself

- `LICENSE`, because GitHub cannot use a default license file. Each repository carries its own, so it ships in clones and downloads. The `LICENSE` here covers this repository only.
- `CODEOWNERS`, because it applies only to the repository it sits in.
- `README.md`.

## The Renovate preset

`default.json` is a [Renovate shared preset](https://docs.renovatebot.com/config-presets/) that extends `config:best-practices`. It does the following:

- Merges minor, patch, digest, pin and lock-file updates automatically once checks pass. Major updates and Go toolchain minor versions wait for manual approval.
- Opens vulnerability fixes with a `security` label and never merges them automatically.
- Groups pins that move together into one pull request, such as a Go toolchain version and its per-architecture checksums.
- Tracks version and checksum pins that no built-in manager reads, such as those in Dockerfiles, workflow files and `go.mod`, through custom managers.

`org-inherited-config.json` applies the preset to every cplieger repository. It extends `github>cplieger/.github`, which Renovate resolves to `default.json`. The self-hosted Renovate runner for the cplieger repositories reads it through Renovate's [`inheritConfig`](https://docs.renovatebot.com/self-hosted-configuration/#inheritconfig) option, so a cplieger repository needs no `renovate.json` of its own.

Another account can extend `github>cplieger/.github` too. The preset's rules also cover cplieger's own packages and repositories, such as the `@cplieger/web-terminal-engine` and `@cplieger/web-terminal-ui` npm packages and the docker-caddy build stages. To leave those out, copy the general rules into your own preset.

The reusable workflows and the lint and format configs the repositories share live in [cplieger/ci](https://github.com/cplieger/ci).

This project was built with AI-assisted tooling using [Claude](https://claude.com), [GPT](https://openai.com), and [Kiro](https://kiro.dev). The human maintainer defines architecture, supervises implementation, and makes all final decisions.
