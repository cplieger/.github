# .github

Account-wide community-health defaults for `cplieger` repos. GitHub applies
these to any public repo that does not provide its own copy — so they live here
once instead of being duplicated. Precedence: a repo's own file wins, otherwise
GitHub falls back to the matching file here.

Inherited defaults (apply to every public repo without its own copy):

- `CODE_OF_CONDUCT.md` — Contributor Covenant v3.0
- `CONTRIBUTING.md` — contribution + commit conventions
- `SECURITY.md` — private vulnerability reporting
- `SUPPORT.md` — where to get help
- `.github/FUNDING.yml` — Sponsor button (placeholder, disabled)
- `.github/PULL_REQUEST_TEMPLATE.md`
- `.github/ISSUE_TEMPLATE/` — bug + feature forms and chooser `config.yml`

Repo-local only (GitHub does **not** inherit these from `.github`):

- `LICENSE` — must live in each repo so it ships in clones/tarballs; the copy
  here covers this repo only.
- `CODEOWNERS` — only governs this repo; each repo needs its own.
- `README.md` — per-repo.

The Renovate preset (`default.json`) lives in this repo, which is why repos
extend `github>cplieger/.github`. The reusable workflows and lint/format configs
live separately in [`cplieger/ci`](https://github.com/cplieger/ci).

This project was built with AI-assisted tooling using [Claude Opus](https://www.anthropic.com/claude) and [Kiro](https://kiro.dev). The human maintainer defines architecture, supervises implementation, and makes all final decisions.
