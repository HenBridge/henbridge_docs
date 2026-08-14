# Bug Bounty Program (post-M1)

This document specifies HenBridge's bug bounty program for the **deployed system**
(post-M1, once `henbridge-web` and `henbridge-contract` ship code to testnet).
Until then, design-level reports follow [SECURITY.md](../SECURITY.md).

HenBridge is a pre-mainnet health-data Digital Public Good with a **constrained
budget**, so this program is deliberately scoped to what we can actually pay
out and run — not a max-bounty enterprise program.

## Scope

### In scope

- `henbridge-web` (authenticated + public emergency page routes, API layer,
  Supabase integration, session/RLS boundaries)
- `henbridge-contract` (Soroban attestation-registry and attester-registry
  crates, on-chain attestation record shape)
- The `henbridge-docs` **design decisions** that shape security posture:
  [data model](../docs/data-model.md), [threat model](../docs/threat-model.md),
  [privacy design](../docs/privacy-design.md) — design-level reports welcome
  (see SECURITY.md).

### Out of scope

- Attacks requiring physical coercion of a patient or health worker at the
  scene (noted in the threat model as out of scope for a technical program).
- Social-engineering a **coerced or bribed attester** — this is a
  governance/dispute-resolution problem (see Residual Risk in the threat
  model), not a code vulnerability.
- DoS on testnet infrastructure during active development (rate limiting is
  the planned mitigation; report only if the edge protection itself is bypassed).
- Third-party dependencies outside our control (report upstream; we track
  them via the [dependency risk process](../docs/dependency-risk.md)).

## Severity tiers & reward ranges

Budgets are **indicative** for a pre-mainnet DPG and will be revisited at
M4 (mainnet + funding). Rewards are paid in USDC on Stellar where legally
permissible; otherwise via the platform's standard payout rail.

| Severity | Definition | Indicative reward (testnet phase) |
| --- | --- | --- |
| **Critical** | Full_profile disclosure, forged attestation, theft/drain of CHW payout pool | \$500 – \$2,000 |
| **High** | Spoofed attester, elevation of privilege (CHW → attester allowlist), emergency-subset tampering that survives re-attest | \$150 – \$500 |
| **Medium** | Information disclosure in a non-emergency path, repudiation gap, broken rate limit | \$50 – \$150 |
| **Low** | Minor privacy weakening, missing header, doc/design flaw with no direct exploit | \$10 – \$50 |
| **Informational** | Hardening suggestion, no exploit | Swag / attribution only |

Severity is assessed against the [threat model](../docs/threat-model.md)
assets and STRIDE entries. A report that only affects testnet data (no real
PHI) is capped at the Medium tier until mainnet.

## Disclosure timeline

- **Researcher notice period:** 90 days from triage acceptance before
  public disclosure.
- **Triage SLA:** initial response within 5 business days; severity
  acknowledged within 10 business days.
- **Fix SLA by severity:** Critical 14 days · High 30 days · Medium 60
  days · Low next scheduled release.
- **Safe harbor:** good-faith research that does not access real patient
  data, does not attack third parties, and stops at proof-of-concept is
  not pursued.
- Reports go through a **GitHub Security Advisory** on the affected repo
  (not public issues) per SECURITY.md.

## Launch checklist

- [ ] Program merged into SECURITY.md and linked here
- [ ] Testnet-phase reward ranges approved by governance/committee
- [ ] Security Advisory template configured on `henbridge-web` and
      `henbridge-contract`
- [ ] This document revisited at M4 for mainnet-tier budgets
