# Security Policy

## Reporting a vulnerability

Please report security issues **privately** — do not open a public issue.

On the affected repository, open the **Security** tab, then click
**"Report a vulnerability"** to open the private advisory form (GitHub
[private vulnerability reporting](https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing-information-about-vulnerabilities/privately-reporting-a-security-vulnerability)
is enabled on all public repositories).

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
  and ship a build-provenance attestation and an attested SBOM. Verify:

  ```sh
  cosign verify ghcr.io/cplieger/<image>:<tag> \
    --certificate-identity-regexp '^https://github.com/cplieger/' \
    --certificate-oidc-issuer https://token.actions.githubusercontent.com
  gh attestation verify oci://ghcr.io/cplieger/<image>:<tag> --owner cplieger
  ```

- **npm / JSR packages** (`@cplieger/*`) are published through GitHub OIDC
  trusted publishers, which attach a Sigstore provenance attestation. Verify
  with `npm audit signatures` (npm) or the provenance link shown on the JSR
  package page.

- **Go modules** are distributed by Git tag and the Go checksum database
  (`sum.golang.org`), with hashes recorded in `go.sum`. Integrity is verified
  automatically by the Go toolchain (`GOFLAGS=-mod=readonly`, `go mod verify`).
  Go modules are not separately signed, so author-identity verification beyond
  the checksum database is not currently available.

## Secrets management

- Publishing uses GitHub **OIDC trusted publishers** (npm/JSR) and **keyless
  cosign** signing, so there are no long-lived registry or signing tokens to
  store or leak.
- The only per-repo CI secret is a Docker Hub token (`DOCKERHUB_TOKEN`) for
  repos whose image dual-publishes to Docker Hub. It is stored as a
  repository-scoped GitHub Actions encrypted secret and is rotated via the
  Docker Hub PAT on suspected exposure.
- Deployment secrets are **age-encrypted at rest in Git** and decrypted only at
  deploy time; plaintext secrets are never committed.
- `gitleaks` runs in CI and GitHub secret-scanning push protection is enabled
  to catch accidental commits of credentials.

## Dependency management

- Dependencies are pinned: Go modules via `go.sum`, npm/JSR via lockfiles,
  GitHub Actions by commit SHA, and container base images by digest.
- [Renovate](https://docs.renovatebot.com/) (configuration inherited from this
  repository's shared preset) opens and auto-merges dependency, action, and
  base-image updates on green CI, keeping the supply chain current.
- New dependencies are selected for necessity, an OSI-compatible license, and
  active maintenance; the standard library is preferred where practical.
- Supply-chain risk is monitored continuously: Trivy and Dependabot alerts feed
  remediation (via Renovate bumps), and CodeQL covers first-party code.
