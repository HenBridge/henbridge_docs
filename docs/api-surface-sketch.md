# API & Contract Surface Sketch (forward-looking)

> This is a design sketch, not an implementation. Nothing described here
> exists in code yet — it exists so `henbridge-web` and `henbridge-contract` are
> built against a shared plan instead of diverging. Update this doc, don't
> silently deviate from it, if the real implementation differs.

## `henbridge-web` API (sketch)

| Method | Path | Auth | Purpose |
| --- | --- | --- | --- |
| `POST` | `/api/profile` | Patient session | Create/update the full profile |
| `GET` | `/api/profile/me` | Patient session | Read own full profile |
| `PATCH` | `/api/profile/me/visibility` | Patient session | Toggle per-field public visibility |
| `GET` | `/e/{public_id}` | None | Public emergency page (renders emergency subset + verified indicator) |
| `POST` | `/api/attestations` | Attester session | Submit an attestation for a record hash |
| `GET` | `/api/attestations/{public_id}` | None | Check current attestation status for a record |

## `henbridge-contract` Soroban interface (sketch)

```rust
pub fn attest(env: Env, attester: Address, record_hash: BytesN<32>) -> u64; // returns ledger timestamp
pub fn get_attestation(env: Env, record_hash: BytesN<32>) -> Option<AttestationRecord>;
pub fn add_attester(env: Env, governance: Address, attester: Address);
pub fn remove_attester(env: Env, governance: Address, attester: Address);
```

This mirrors the
[attestation record schema](data-model.md#attestation-record-on-chain-soroban).
Any change to function names or argument order here is a shared-contract
change per the root README.
