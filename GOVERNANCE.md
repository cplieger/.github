# Governance

This document describes how the `cplieger` open-source projects are governed.
It is the shared governance model for every public repository in the account
(libraries, container images, applications, and shared CI). Individual repos
may add a repo-specific note, but this document is the default and is
authoritative unless a repo overrides it.

This model is intentionally lightweight. It is an honest description of how the
projects are actually run today — a single maintainer — rather than an
aspirational structure that does not match reality. It follows the spirit of
[CNCF's Maintainer Council template](https://contribute.cncf.io/projects/best-practices/governance/templates/governance-maintainer)
and GitHub's
[Minimum Viable Governance](https://github.blog/2021-07-22-minimum-viable-governance-lightweight-community-structure-foss-projects/),
scaled down to a single-maintainer project.

## Roles and responsibilities

| Role | Who | Responsibilities |
|---|---|---|
| **Maintainer** | Christopher Plieger ([@cplieger](https://github.com/cplieger)) | Sole decision-maker. Reviews and merges changes, cuts releases, triages issues and security reports, sets technical direction, and administers repository settings, secrets, and branch protection. |
| **Contributor** | Anyone who opens an issue or pull request | Proposes changes via pull request, reports bugs and vulnerabilities, and participates in discussion. Contributors have no merge rights. |

There is currently one maintainer. The maintainer holds every project role.
This is recorded plainly so contributors know who decides and how.

## Decision-making

Decisions are made by the maintainer. In practice:

- **Changes** land through pull requests. Every change is gated by the required
  `ci / validate` status check before it can merge.
- **Bugs and features** are tracked as GitHub Issues. The maintainer triages and
  prioritizes them.
- **Disagreements** are resolved by discussion in the relevant issue or pull
  request; the maintainer makes the final call.
- **Security reports** follow the private process in
  [SECURITY.md](./SECURITY.md).

## Becoming a maintainer

These projects welcome additional maintainers. A contributor who provides
sustained, high-quality contributions and demonstrates good judgment may be
invited by the maintainer to become a co-maintainer with merge and release
rights. Adding a second maintainer is also part of the continuity plan; see
[CONTINUITY.md](./CONTINUITY.md).

## Changes to this document

This governance model may change as a project grows (for example, when a second
maintainer joins). Changes are made by pull request to this repository and take
effect on merge.

## Code of conduct

All participation is governed by the [Code of Conduct](./CODE_OF_CONDUCT.md).

## License

Unless a repository states otherwise, these projects are licensed under
**GPL-3.0-or-later**. Contributions are accepted under the same license.
