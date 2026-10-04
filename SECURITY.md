# Security policy

This policy covers every cplieger repository that has no security policy of its own.

## Reporting a vulnerability

Report a vulnerability privately. Do not open a public issue, pull request or comment about it.

1. Open the affected repository on GitHub and select the **Security** tab.
2. Select **Report a vulnerability**. GitHub opens a private form. Only you, the maintainer and the people the maintainer adds to your report can read it. GitHub's guide to [reporting a vulnerability privately](https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing-information-about-vulnerabilities/privately-reporting-a-security-vulnerability) shows each step.
3. Describe the affected version or commit, the steps that reproduce the problem and what an attacker could do with it.

A private repository, an archived repository or a fork has no **Report a vulnerability** button. If the repository you're reporting on has none, use the [form on cplieger/.github](https://github.com/cplieger/.github/security/advisories/new) and name the repository in your report.

Some repositories package another project's software, such as the program a container image runs. If the problem is in that software, please report it to the upstream project too.

## What happens after you report

You'll get a reply within 7 days. The fix is prepared in private and released before the vulnerability is made public, normally within 90 days of your report.

## Supported versions

A repository that publishes releases gets security fixes in its latest release only, unless its README or its own security policy names other supported versions. Older releases, including older major versions, do not get them. To get a fix, upgrade to the latest release. A repository that publishes no releases gets fixes on its default branch. In a repository whose version is below 1.0, a minor version can contain breaking changes, so read the release notes before you upgrade.

## Verifying a release

How you check a release depends on what the repository publishes.

Container images are published to `ghcr.io/cplieger/<image>` and `docker.io/cplieger/<image>`. A release is the same image in both registries. A pre-release version, such as `v1.2.3-dev.4`, is published to `ghcr.io` only. Every image is signed with [cosign](https://github.com/sigstore/cosign) when it's built. Each image also carries a signed SBOM, the list of software inside it, and a record of how it was built. To check the signature and the SBOM, run:

```sh
cosign verify ghcr.io/cplieger/<image>:<tag> \
  --certificate-identity-regexp '^https://github.com/cplieger/ci/\.github/workflows/docker-release\.yaml@' \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com
cosign verify-attestation --type spdxjson ghcr.io/cplieger/<image>:<tag> \
  --certificate-identity-regexp '^https://github.com/cplieger/ci/\.github/workflows/docker-release\.yaml@' \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com
```

To read the build record, run `docker buildx imagetools inspect ghcr.io/cplieger/<image>:<tag> --format '{{ json .Provenance }}'`.

npm packages, named `@cplieger/<package>`, are published from GitHub Actions with a [provenance statement](https://docs.npmjs.com/generating-provenance-statements). It links each version to the commit and the build that produced it. In a project that installs one, run `npm audit signatures` to check it.

Go modules are published as Git tags. By default the Go toolchain checks every module it downloads against the [Go checksum database](https://sum.golang.org). Run `go mod verify` to check the copies already on your machine. Go modules carry no separate signature.
