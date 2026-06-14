# Roadmap (default)

This is the default roadmap for the `cplieger` repositories. It applies to the
mature libraries, container images, and tools whose scope is stable. Actively
evolving applications keep a repo-specific `ROADMAP.md` that takes precedence
(currently: `subflux`, `vibekit`, `vibecli`).

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
  [gremlins](https://gremlins.dev/) mutation-testing run are triaged into fixes
  and new regression tests. This is the main source of ongoing change in the
  mature libraries.
- **Security posture.** CodeQL, Trivy, OpenSSF Scorecard, and gitleaks findings
  are addressed as they arise; the projects track the OpenSSF Best Practices
  badge criteria.
- **Bug and security response.** Reported issues and vulnerabilities are
  triaged and fixed per [SECURITY.md](./SECURITY.md).

## Repositories with a TypeScript surface

For repositories that ship or build TypeScript (the TS libraries and the
applications with a web frontend): **adopt the stable `tsgo`
([@typescript/native-preview](https://www.npmjs.com/package/@typescript/native-preview))
release once it reaches general availability**, replacing the pinned preview
currently used for typechecking.

## Scope changes

New features are considered on their merits via GitHub Issues. Because the
libraries deliberately keep a minimal surface, scope additions are the exception,
not the default; see each library's "Unsupported by Design" notes where present.
