# Data Retention Policy

## Off-chain data (Supabase)

| Data | Retention | Deletion |
| --- | --- | --- |
| Active patient profile | Retained while the account is active | Deleted on verified deletion request, subject to legal hold requirements under NDPA 2023 |
| Inactive account | Flagged after 24 months of no login; patient notified before deletion | Deleted after notice period unless the patient re-engages |
| Audit/access logs | Retained 12 months for security investigation purposes | Rotated out automatically |
| Scan-event analytics | Retained 12 months for security and trend analysis | Automatically deleted or aggregated; see [analytics-spec.md](analytics-spec.md) |

## On-chain data (Soroban)

Attestation records (hash, attester, timestamp) are, by the nature of a
blockchain, **not deletable**. This is a known and accepted tradeoff:

- The hash alone reveals nothing about the underlying record without also
  having the original data
- If a patient wants an attestation invalidated, the underlying off-chain
  record is changed (invalidating the hash match) and, where the governance
  process supports it, a dispute is filed to flag the stale on-chain entry —
  see the root README's [Dispute & Governance](../README.md#attestation--trust-model)
  notes
- No retention period applies to on-chain data because no reversible
  personal data is ever written there in the first place

## Backups

Off-chain backups follow the same retention ceiling as live data; a deletion
request is not complete until it has propagated to backups within the
documented backup retention window (to be finalized alongside `henbridge-web`'s
infrastructure choices).
