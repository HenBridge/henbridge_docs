# Security Policy

## Scope

This repository (`lafiya-docs`) contains documentation only — no application or contract code lives here. There is nothing here to have a code-level vulnerability, but the design decisions in this repo (the [data model](docs/data-model.md), [threat model](docs/threat-model.md), and [privacy design](docs/privacy-design.md)) shape the security posture of every other Lafiya repo, so design-level reports are welcome and taken seriously.

The full bug bounty program specification is documented in
[docs/bug-bounty-program.md](docs/bug-bounty-program.md).

- A gap or incorrect assumption in the [threat model](docs/threat-model.md)
- A flaw in the proposed attestation scheme or the on-chain/off-chain boundary described in [privacy-design.md](docs/privacy-design.md)
- A privacy issue in the [data model](docs/data-model.md) — e.g. a field proposed for the public emergency subset that shouldn't be there

Lafiya operates a formal bug bounty program for all repositories in the
Lafiya ecosystem. See the [program specification](docs/bug-bounty-program.md)
for:

Please report privately via a **GitHub Security Advisory** on this repository rather than a public issue, so the report can be reviewed before any sensitive detail is public.

## Reporting vulnerabilities in the running application or contracts

Once `lafiya-web` and `lafiya-contract` exist and ship code, report vulnerabilities in those systems in their own repositories, following their respective `SECURITY.md` files.

## Supported versions

Lafiya is pre-alpha, Stellar **testnet** only. No production deployment
exists yet, so there is no supported-version matrix to publish.

## Bug bounty program

Once `lafiya-web` and `lafiya-contract` ship code, a formal
[bug bounty program](docs/bug-bounty.md) defines scope, severity tiers, and
the disclosure timeline for the deployed system. Design-level reports to this
repo remain welcome per "What to report here" above.
