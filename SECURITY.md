# Security Policy

## Reporting a vulnerability

Please report security issues **privately**; do not open a public issue.

On the affected repository, open the **Security** tab, then click
**"Report a vulnerability"** to open the private advisory form (GitHub
[private vulnerability reporting](https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing-information-about-vulnerabilities/privately-reporting-a-security-vulnerability)
is enabled on every first-party repository). If the affected repository has no
such button, which is the case for an archived repository and for a fork of an
upstream project, file the report on the
[`cplieger/.github`](https://github.com/cplieger/.github/security/advisories/new)
Security tab instead and name the affected repository in the report.

Include the affected version/commit, reproduction steps, and impact. You will
receive an acknowledgement within **7 days**. Reports are handled under a
coordinated disclosure process: a fix is prepared privately and released before
public disclosure, normally within **90 days** of the initial report.

## Supported versions

Only the latest released version of each project is supported. Pre-1.0 (`0.x`)
releases may contain breaking changes between minor versions.

## Verifying releases

How to verify the integrity and authenticity of a release depends on the
artifact type:

- **Container images** (`ghcr.io/cplieger/<image>`, `docker.io/cplieger/<image>`)
  are signed with [cosign](https://github.com/sigstore/cosign) keyless (OIDC)
  and ship an attested SBOM and a BuildKit SLSA provenance attestation. Verify:

  ```sh
  cosign verify ghcr.io/cplieger/<image>:<tag> \
    --certificate-identity-regexp '^https://github.com/cplieger/' \
    --certificate-oidc-issuer https://token.actions.githubusercontent.com
  docker buildx imagetools inspect ghcr.io/cplieger/<image>:<tag> \
    --format '{{ json .Provenance }}'
  ```

- **npm / JSR packages** (`@cplieger/*`) are published through GitHub OIDC
  trusted publishers, which attach a Sigstore provenance attestation. Verify
  with `npm audit signatures` (npm) or the provenance link shown on the JSR
  package page.

- **Go modules** are distributed by Git tag and the Go checksum database
  (`sum.golang.org`), with hashes recorded in `go.sum`. Integrity is verified
  automatically by the Go toolchain, and on demand with `go mod verify`.
  Go modules are not separately signed, so author-identity verification beyond
  the checksum database is not currently available.

## Secrets management

- Publishing uses GitHub **OIDC trusted publishers** (npm/JSR) and **keyless
  cosign** signing, so there are no long-lived registry or signing tokens to
  store or leak.
- Image repos hold a Docker Hub username and token (`DOCKERHUB_USERNAME`,
  `DOCKERHUB_TOKEN`) for the Docker Hub half of a dual publish, stored as
  repository-scoped GitHub Actions encrypted secrets; the token is rotated as a
  Docker Hub PAT on suspected exposure. Library repos hold no secrets.
  `cplieger/ci`, which drives every repo's pipelines, additionally holds the
  scoped GitHub tokens its release, sync, and audit automation needs.
- Deployment secrets are **age-encrypted at rest in Git** and decrypted only at
  deploy time; plaintext secrets are never committed.
- `gitleaks` runs in CI and GitHub secret-scanning push protection is enabled
  to catch accidental commits of credentials.

## Dependency management

- Dependencies are pinned: Go modules via `go.sum`, npm/JSR via lockfiles,
  GitHub Actions by commit SHA, and container base images by digest.
- [Renovate](https://docs.renovatebot.com/) (configuration inherited from this
  repository's shared preset) opens dependency, action, and base-image updates
  and auto-merges the routine ones, meaning minor, patch, and digest bumps, on
  green CI. Major updates and security-advisory bumps are reviewed by hand.
- New dependencies are selected for necessity, an OSI-compatible license, and
  active maintenance; the standard library is preferred where practical.
- Supply-chain risk is monitored continuously: Trivy and Dependabot alerts feed
  remediation (via Renovate bumps), and CodeQL covers first-party code.
