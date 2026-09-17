# Roadmap (default)

This is the default roadmap for the `cplieger` repositories. It applies to the
mature libraries, container images, and tools whose scope is stable. An
actively evolving application may keep a repo-specific `ROADMAP.md` that takes
precedence; none does today.

Most of these projects are **feature-complete and in maintenance mode**: they do
one thing, the public surface is intentionally small and stable, and "done" is a
valid state. The roadmap for such a project is therefore primarily about staying
healthy rather than growing scope.

## Ongoing across all repositories

- **Dependency and toolchain currency.** Renovate keeps dependencies, base
  images, and pinned GitHub Actions up to date; Go and Node toolchain versions
  track upstream stable releases.
- **Quality from continuous testing.** Findings from the weekly central
  [fuzzing](https://go.dev/doc/security/fuzz/) run and the weekly
  mutation-testing runs ([gremlins](https://gremlins.dev/) for Go,
  [Stryker](https://stryker-mutator.io/) for TypeScript) are triaged into fixes
  and new regression tests.
- **Security posture.** CodeQL, Trivy and gitleaks findings are addressed as
  they arise.
- **Bug and security response.** Reported issues and vulnerabilities are
  triaged and fixed per [SECURITY.md](./SECURITY.md).

## Scope changes

New features are considered on their merits via GitHub Issues. Because the
libraries deliberately keep a minimal surface, scope additions are the exception,
not the default; see each library's "Unsupported by Design" notes where present.
