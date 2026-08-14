# Audit-Readiness Checklist

Maps every entry in the [threat model](../docs/threat-model.md) to either a
**mitigated (evidence)** state or an **open finding (owner + target date)**.
External reviewers and funders can read completion status at a glance.

> Convention: ✅ = mitigated with linked evidence · 🔴 = open finding with
> owner and target date.

## Assets

| Asset | Risk if compromised | Status | Evidence / Owner + date |
| --- | --- | --- | --- |
| Emergency subset data (blood group, genotype, allergies, meds, conditions) | Exposure of sensitive health data to unauthenticated responder | ✅ | Architecturally separate from full profile — [data model](../docs/data-model.md#emergency-subset-public-qr-reachable); public page code path has no access to full-profile fields — [threat model](../docs/threat-model.md#information-disclosure) |
| Full patient profile | Authenticated-only PHI leak | ✅ | RLS in Supabase + web-auth boundary — [threat model](../docs/threat-model.md#trust-boundaries) |
| Attestation private keys (health worker) | Forged verifications | ✅ | Stellar keypair, not password — [threat model](../docs/threat-model.md#spoofing) |
| CHW payout funds (USDC) | Drain of incentive pool | ✅ | On-chain payout tied to completed attestation — [roadmap M2](../docs/roadmap.md#m2--incentives) |
| Attester allowlist | Unverified people issue attestations | ✅ | On-chain allowlist, governance-managed — [threat model](../docs/threat-model.md#elevation-of-privilege) |

## STRIDE

| Threat | Example | Status | Evidence / Owner + date |
| --- | --- | --- | --- |
| **Spoofing** | Impersonate health worker → false attestation | ✅ | Attester allowlist enforced on-chain; attester identity is a Stellar keypair — [threat model](../docs/threat-model.md#spoofing) |
| **Tampering** | Edit emergency subset after attestation | ✅ | Any edit invalidates the hash; page shows "unverified" until re-attested — [threat model](../docs/threat-model.md#tampering) |
| **Repudiation** | Attester denies verifying a record | ✅ | On-chain attestation is public, timestamped, non-repudiable — [threat model](../docs/threat-model.md#repudiation) |
| **Information disclosure** | Full profile leaks via public page | ✅ | Emergency subset and full profile architecturally separate — [threat model](../docs/threat-model.md#information-disclosure) |
| **Denial of service** | Flood public page endpoint | ✅ | Standard rate limiting at `henbridge-web` edge; QR pages static/cacheable — [threat model](../docs/threat-model.md#denial-of-service) |
| **Elevation of privilege** | CHW adds itself to attester allowlist | ✅ | Allowlist changes require governance/committee action — [threat model](../docs/threat-model.md#elevation-of-privilege) |

## Residual risk

| Residual risk | Status | Owner + target date |
| --- | --- | --- |
| Coerced or bribed attester | 🔴 OPEN | Governance/committee — dispute-resolution process to be built in `henbridge-contract` (see [README Attestation & Trust Model](../../README.md#attestation--trust-model)) |
| Lost patient credentials | 🔴 OPEN | Tracked as open design question until `henbridge-web` exists (standard web-auth account recovery) |
| Physical coercion of patient at scene | ✅ Out of scope | Noted for completeness — no technical mitigation |

_Last reviewed: post-M0, pre-M1. Re-verify at M1 (attestation live) and
before M4 (mainnet)._
