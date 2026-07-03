# Project continuity and access recovery

This document is an honest description of project continuity across the
`cplieger` repositories: what keeps the projects healthy if work pauses, and —
candidly — where the single-maintainer risk currently sits. It exists so that
users and potential contributors can make an informed judgement, and so the
maintainer has a written plan to act on.

It corresponds to the OpenSSF Best Practices `access_continuity` criterion.
**That criterion is not yet fully met** (see "Current gap" below); this document
states the real situation rather than overclaiming.

## What reduces continuity risk today

Several properties mean these projects degrade gracefully even without active
day-to-day attention:

- **Dependencies stay current automatically.** [Renovate](https://docs.renovatebot.com/)
  opens and auto-merges dependency and base-image update PRs on green CI, so
  the projects do not rot between releases.
- **Releases are fully automated and credential-light.** The release pipeline
  (`cplieger/ci`) builds, versions (git-cliff), signs (cosign), attests an SBOM,
  and publishes. Library publishing uses OIDC trusted publishers (npm/JSR) — there
  are no long-lived publish tokens to lose or rotate.
- **Everything is in git.** Source, CI configuration, and deployment config are
  all version-controlled and reproducible. There is no undocumented build state.
- **Security scanning is continuous and unattended.** CodeQL, Trivy, OpenSSF
  Scorecard, and gitleaks run on schedule and on every push.

## Current gap (honest)

The maintainer is currently the only person with administrative access to the
GitHub account, the project's domain (at the registrar), and release
infrastructure. **No second individual currently holds emergency access.** If
the maintainer were permanently unavailable, no one could merge changes or cut
new releases until access was recovered through the registrar's and GitHub's
account-recovery and estate processes. For that reason the `access_continuity`
criterion is marked **Unmet** rather than met.

## Continuity plan

The trigger for a heavier-weight plan is **meaningful external adoption** of a
project. When a project reaches that point, the maintainer will:

1. **Add a second owner.** Invite a trusted second person — a developer family
   member, or a sustained community contributor (see
   [GOVERNANCE.md](./GOVERNANCE.md) "Becoming a maintainer") — as a
   co-maintainer and GitHub account/org owner, so issues can be triaged,
   changes merged, and releases cut by more than one person.
2. **Escrow recovery material.** Place the credentials needed to recover the
   GitHub account, the domain registrar, and any signing material in a password
   manager with a configured emergency-access contact, accompanied by written
   instructions (and, where relevant, estate/legal provisions granting the
   successor the right to continue the project — the "lockbox and will" model
   the OpenSSF criterion describes for solo projects).

Until those steps are taken, this document is the public record of the gap and
the intended remedy.
