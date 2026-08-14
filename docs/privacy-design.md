# HenBridge Privacy Design

## Data minimization & consent

The public emergency page shows only the
[emergency subset](data-model.md#emergency-subset-public-qr-reachable) —
never the full profile. Every field in the emergency subset has an
independent visibility toggle; a patient can, for example, publish blood
group and genotype while hiding medications. Defaults favor safety (blood
group and genotype default on) but every field is patient-controlled and
reversible.

## NDPA 2023 compliance mapping

The Nigeria Data Protection Act (2023) governs all personal data HenBridge
holds. Key obligations and how the design meets them:

| NDPA principle | HenBridge's design response |
| --- | --- |
| Lawful basis / consent | Explicit, granular, per-field opt-in for the public page; withdrawable at any time |
| Purpose limitation | Emergency subset exists solely for emergency treatment context; no secondary use (marketing, resale) |
| Data minimization | Public page schema is deliberately the smallest set of fields that change emergency treatment |
| Storage limitation | See [data-retention-policy.md](data-retention-policy.md) |
| Integrity & confidentiality | Encryption at rest, Row-Level Security, no health data on any public ledger |
| Accountability | This document, the threat model, and the ADR log are the accountability trail for design decisions |

## On-chain / off-chain boundary

This is HenBridge's central privacy guarantee: **no personal health data ever
touches the blockchain.** The chain holds exactly three things — a hash, an
attester identity, and a timestamp (see
[data-model.md](data-model.md#attestation-record-on-chain-soroban)). This is
a one-way design choice, not a configuration flag: a hash cannot be reversed
to recover the underlying record, and the off-chain database is the only
place the emergency subset or full profile is ever stored in readable form.

This boundary is why Stellar is a *core* component of HenBridge rather than an
add-on — see [ADR 0001](adr/0001-why-stellar.md) and
[ADR 0002](adr/0002-off-chain-data-on-chain-attestation.md).

## Data subject rights

Patients can, at minimum:

- View everything HenBridge holds about them
- Correct inaccurate fields
- Toggle what's public
- Request deletion (subject to the retention rules in
  [data-retention-policy.md](data-retention-policy.md) — note that an
  on-chain attestation hash cannot be deleted, only invalidated by the
  underlying record changing)
