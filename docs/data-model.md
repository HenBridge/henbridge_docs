# HenBridge Data Model

This is the source of truth for every field HenBridge stores, where it lives,
and who can see it. `henbridge-web`'s database schema and `henbridge-contract`'s
on-chain structs must match this document — see the root README's
[Shared Contracts](../README.md#shared-contracts-must-stay-in-sync-across-repos)
section.

## Emergency subset (public, QR-reachable)

This is the *only* data exposed on the unauthenticated public emergency page:

| Field | Type | Notes |
| --- | --- | --- |
| `name` | string | First name + last initial by default; full name is the patient's choice |
| `age` | integer | Derived from date of birth; date of birth itself is not exposed |
| `photo_url` | string (URL) | Optional; helps a responder confirm identity |
| `blood_group` | enum | `A+`, `A-`, `B+`, `B-`, `AB+`, `AB-`, `O+`, `O-`, `unknown` |
| `genotype` | enum | `AA`, `AS`, `SS`, `AC`, `SC`, `unknown` |
| `allergies` | string[] | Free-text list, patient-entered |
| `medications` | string[] | Emphasis on anticoagulants, insulin, anti-epileptics |
| `conditions` | string[] | Chronic conditions / implants |
| `emergency_contacts` | `{ name, relationship, phone }[]` | At least one recommended, not required |
| `language` | string | Preferred spoken language |
| `attestation` | see below | Verified indicator, if any |

## Full patient profile (private, authenticated)

Everything in the emergency subset, plus:

| Field | Type | Notes |
| --- | --- | --- |
| `date_of_birth` | date | Used to derive public `age` only |
| `full_medical_history` | text/document[] | Never exposed publicly |
| `facility_visits` | record[] | Referral and visit history, if the patient chooses to log it |
| `contact_email` / `contact_phone` | string | Login and account recovery only |
| `consent_flags` | object | Per-field publish/hide toggles — see [privacy-design.md](privacy-design.md) |

## Attestation record (on-chain, Soroban)

Only this shape is ever written to the chain — never the fields above:

```text
AttestationRecord {
  record_hash: BytesN<32>,   // hash of the emergency subset at attestation time
  attester: Address,         // the verifying health worker's Stellar address
  timestamp: u64,            // ledger time of attestation
}
```

A responder's client re-hashes the emergency subset it just fetched and
checks it against `record_hash` for the matching `attester` on-chain — that
match is what produces the "verified" indicator. See
[ADR 0002](adr/0002-off-chain-data-on-chain-attestation.md) for why only a
hash is ever published.

## Versioning

Any change to a field name, type, or enum value in this document is a
**shared-contract change**: it must be reflected in `henbridge-web`'s schema
and, if it touches the attestation record, in `henbridge-contract`'s Rust
struct, in the same change set or a tracked follow-up in each repo.
