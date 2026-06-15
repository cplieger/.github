# Security assurance case (default)

This document explains why the `cplieger` projects are considered _adequately
secure_ for their intended use. It is the shared (default) assurance case for
every public repository; repositories with a larger or more sensitive attack
surface (for example `auth`, `ssrf`, `cert-converter`, `docker-age`,
`pg-autodump`, `subflux`, `vibekit`, `vibecli`) carry a repo-specific
`docs/assurance-case.md` that extends this one with details of their own
threat model.

It is structured as a Claims-Arguments-Evidence (CAE) assurance case, following
the model of the
[OpenSSF BadgeApp assurance case](https://github.com/coreinfrastructure/best-practices-badge/blob/main/docs/assurance-case.md).
It is honest: it states residual risks rather than claiming perfection.

## Top-level claim

**Each project is adequately secure for its intended use** — primarily
self-hosted libraries, tools, and services for a private homelab and their
public open-source consumers — **given its threat model and the value of the
assets it handles.**

## Threat model (summary)

- **In scope:** untrusted network input to services and parsers; malicious or
  malformed data from third-party APIs; supply-chain risk from dependencies and
  base images; accidental secret leakage; tampering of release artifacts in
  transit.
- **Out of scope / accepted:** a fully resourced targeted attacker against the
  single maintainer's accounts; base-image CVEs with no upstream fix yet
  (tracked, see below); and single-maintainer continuity risk (see
  [CONTINUITY.md](./CONTINUITY.md)).

## Argument: defense-in-breadth across the lifecycle

Security is addressed in each software lifecycle process, not bolted on at one
stage.

### Design

- Memory-safe languages throughout (Go, TypeScript) — whole classes of memory
  corruption vulnerabilities do not apply.
- Least privilege at runtime: container images are distroless and run as a
  non-root user wherever the workload allows.
- Outbound requests that touch user-supplied URLs are validated against SSRF
  (private/loopback/link-local/transition ranges blocked, connected IP
  re-validated) via the `ssrf` library.
- Secrets are never committed: deployment secrets are age-encrypted in git and
  decrypted only at deploy time; publishing uses OIDC trusted publishers, so no
  long-lived registry tokens exist.

### Implementation

- No home-grown cryptography: only vetted standard-library / `golang.org/x/crypto`
  primitives are used; passwords (where stored) use Argon2id with per-user salt.
- Inputs from untrusted sources are validated and bounded (size limits on reads,
  parser hardening, path-traversal checks on file writes).
- Hardened HTTP defaults (explicit timeouts, bounded bodies).

### Verification

- **Static analysis:** CodeQL (SAST) and `golangci-lint` / ESLint + strict
  typechecking run on every push.
- **Dependency / CVE scanning:** Trivy (filesystem and image) reports to the
  Security tab; Dependabot alerts and private vulnerability reporting are enabled.
- **Secret scanning:** gitleaks in CI, plus GitHub secret scanning with push
  protection.
- **Dynamic / property testing:** the Go race detector runs on every test;
  property-based tests (`rapid` / `fast-check`) and Go native fuzzing exercise
  parsers, validators, and sanitizers; a weekly central fuzz run explores deeper
  and files regressions as committed seed corpora.
- **Test adequacy:** weekly [gremlins](https://gremlins.dev/) mutation testing
  measures and drives test-suite strength.
- **Supply-chain integrity:** base images and pinned GitHub Actions are
  SHA-256 digest-pinned; Renovate keeps them current; release images are signed
  with cosign and ship an attested SBOM.

### Operation and response

- Vulnerabilities are reported privately and triaged per [SECURITY.md](./SECURITY.md).
- Releases carry git-cliff-generated notes; fixed vulnerabilities are called out.

## Residual risks (stated honestly)

- **Single-maintainer continuity.** Documented in [CONTINUITY.md](./CONTINUITY.md);
  the `access_continuity` criterion is not yet met.
- **Base-image CVEs.** CVE scanning is advisory, not blocking: base-image
  vulnerabilities are often outside the maintainer's immediate control (no
  upstream fix yet). The fix path is a Renovate base/package bump, not a blocked
  pipeline. Findings are visible in each repo's Security tab.
- **No independent third-party security review.** Review is performed by the
  maintainer with tool assistance, not by an external auditor.
