# Contributing to HenBridge

Thanks for your interest in HenBridge — an open-source Digital Public Good. This
repository (`henbridge-docs`) is documentation only: the concept note, data
model, threat model, privacy design, and funding materials the rest of the
`henbridge` organization is built against. See the [root README](README.md#henbridge-organization)
for where application and contract code live.

## Before you open a PR

For anything beyond a typo or broken link, please open an issue first using
the **Proposal** template. This is especially important for changes to the
[data model](docs/data-model.md) or [attestation record schema](docs/data-model.md#attestation-record-on-chain-soroban) —
these are shared contracts with `henbridge-web` and `henbridge-contract`, and
changing them here without warning breaks other repos silently.

## Making the change

1. Fork and branch from `main`.
2. Follow the [docs style guide](docs/style-guide.md).
3. Run the markdown linter locally if you have Node available:

   ```bash
   npx markdownlint-cli2 "**/*.md"
   ```

   CI runs the same check on every PR.
4. Update [docs/README.md](docs/README.md)'s index if you add a new document.
5. If your change affects a shared contract (data model, attestation schema,
   env/config keys), say so explicitly in the PR description using the
   checklist in the PR template, and open a matching issue in the affected
   repo(s).

## Commit and PR style

- Write commit messages that explain *why*, not just *what*.
- Keep PRs scoped to one topic — a data model change and a typo fix belong in
  separate PRs.

## Code of Conduct

By participating, you agree to abide by the [Code of Conduct](CODE_OF_CONDUCT.md).

## Secure-SDLC policy

Applies to all changes, with stricter rules for security-sensitive ones.

- **Mandatory code review for security-sensitive changes:** any PR touching
  auth, data access (RLS / row policies), or contract logic (`henbridge-contract`)
  requires review by a maintainer familiar with the
  [threat model](docs/threat-model.md) before merge. Self-merge is not
  permitted for these paths.
- **Dependency scanning:** `npm audit` (npm) and `cargo audit` (crates.io)
  run in CI on every PR. New dependencies must pass the
  [third-party dependency risk assessment](docs/dependency-risk.md) — no
  unvetted dependency is merged.
- **Secrets management:** no secrets (keys, mnemonics, `.env` values) may be
  committed. Scan for accidental secrets in CI; anything pushed to git
  history is rotated immediately and the commit purged. Local `.env.example`
  files contain placeholders only.

## Reporting a security or privacy issue

Please don't open a public issue for a security or privacy concern — see
[SECURITY.md](SECURITY.md).
