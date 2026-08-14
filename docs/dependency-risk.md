# Third-Party Dependency Risk Assessment

Process for vetting new dependencies before they enter `henbridge-web` (npm) or
`henbridge-contract` (crates.io), and for re-reviewing existing ones. This is the
companion to the [secure-SDLC policy](../CONTRIBUTING.md#secure-sdlc-policy)
in CONTRIBUTING.md.

## Acceptance criteria for a new dependency

A new dependency is accepted only if **all** of the following hold:

- **Maintenance activity:** ≥1 release in the last 12 months, or explicitly
  pinned to a commit we have reviewed.
- **License compatibility:** MIT/Apache-2.0/BSD/ISC (permissive) or
  compatible with HenBridge's MIT licensing across all three repos. Copyleft
  (GPL/AGPL) is rejected unless isolated in a non-distributed tool.
- **Security history:** no unpatched critical CVE open against the version
  range we would pin.
- **Supply-chain hygiene:** published on the official registry
  (npmjs.org / crates.io), not a git-URL or tarball from an arbitrary host,
  unless the registry package is unavailable and the source is reviewed.

## Red-flag list (reject or escalate)

- Single-maintainer package with no co-maintainers and no org backing.
- Install scripts (`postinstall`) that execute arbitrary code.
- Deprecated, archived, or "looking for maintainer" status.
- Known typosquat of a popular package.
- Native binaries from an unverified publisher.
- Any dependency that introduces a transitive copyleft license into a
  distributed artifact.

## Ongoing re-review cadence

- **npm (`henbridge-web`):** `npm audit` in CI on every PR; `npm outdated`
  reviewed monthly by a designated maintainer; major-version bumps require a
  short ADR if they touch a shared contract.
- **crates.io (`henbridge-contract`):** `cargo audit` in CI on every PR;
  `cargo outdated` reviewed quarterly; security advisories from
  `rustsec` triaged within 5 business days.
- **Renovate/Dependabot:** enabled on both repos with grouped minor/patch
  updates and manual review for majors.

## Responsibilities

- **PR author:** runs the audit command locally, declares new deps in the
  PR description.
- **Reviewer:** confirms the acceptance criteria and absence of red flags
  before approving a security-sensitive change.
- **Governance/committee:** ratifies any exception to the red-flag list.
