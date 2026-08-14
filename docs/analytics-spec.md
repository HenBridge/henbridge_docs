# Analytics Specification

HenBridge tracks page scans on the public emergency page to measure system reach and utility in the field. To balance the need for analytics with patient privacy, this document specifies the scan-event schema, data minimization guidelines, and retention rules.

## Scan-Event Schema

When a user scans a HenBridge Card QR code and requests the public emergency page, the server logs a single analytics event. The schema contains the following fields:

| Field | Type | Description |
| --- | --- | --- |
| `event_id` | UUID | Primary key. A unique identifier for the scan event. |
| `patient_analytics_hash` | string (64-char hex) | A one-way, salted hash of the patient's ID: `SHA-256(patient_id + salt)`. The salt is stored securely on the server. This allows measuring unique cards scanned without revealing the identity of the patient or the raw `record_hash` specified in [data-model.md](data-model.md#attestation-record-on-chain-soroban). |
| `scanned_by` | enum | Represents the scan context role. Values: `responder` (default, anonymous visitor), `patient` (patient viewing their own card while logged in), `chw` (community health worker logged in), `anonymous` (unidentified scan). |
| `referrer_channel` | enum | The access vector. Values: `qr_code`, `nfc`, `manual_search`, `direct_url`. Derived from URL routing parameters (e.g. `?ref=qr`). |
| `timestamp` | timestamp | The UTC date and time of the scan, truncated to hourly resolution (e.g. `2026-07-18T09:00:00Z`). |
| `status` | enum | Outcome of the page load. Values: `success` (page loaded successfully and verification succeeded), `failed_verification` (page loaded but off-chain data did not match the attestation hash), `record_not_found` (no record exists for the scanned identifier). |
| `error_reason` | string | Optional. A short error code when status is not `success` (e.g. `invalid_hash_signature`, `network_error`). |

## Privacy Boundaries & Non-Logged Fields

To comply with the Nigeria Data Protection Act (NDPA) 2023 and HenBridge's central privacy guarantees, the system explicitly drops and does not log the following data:

- **No Patient Health Data (PHI) / Profile Data**: The system never logs the content of the emergency subset (defined in [data-model.md](data-model.md#emergency-subset-public-qr-reachable)) or the full patient profile. This includes name, age, blood group, genotype, allergies, medications, conditions, emergency contacts, and language.
- **No Plaintext Identifiers**: The system never logs raw `patient_id` values, email addresses, phone numbers, or the raw `record_hash`.
- **No IP Addresses or Network Identifiers**: The visitor's IP address is discarded at the server boundary and is never written to the analytics database.
- **No Precise Location Data**: The system never requests or logs GPS coordinates. It may log coarse location (e.g. country or state) derived server-side from IP routing before the IP is discarded, but never precise geolocation.
- **No Device Fingerprints / Raw User-Agents**: Raw `User-Agent` strings are discarded. The server may parse the browser/operating system type into broad, non-identifying categories (e.g., `Mobile`, `Desktop`) but does not log detailed device configurations.
- **No Full Request URLs / Query Strings**: Query parameters beyond the referrer channel (e.g. dynamic state or session parameters) are stripped before logging.

## Data Retention & Aggregation

Scan-event logs are stored off-chain in Supabase and are subject to the following storage limitations:

- **Raw Event Retention**: Raw scan-event records are retained for a maximum of **12 months** for operational and security investigation purposes. This is consistent with the retention period for audit/access logs in [data-retention-policy.md](data-retention-policy.md).
- **Automated Deletion & Aggregation**: After 12 months, raw scan-event records are automatically deleted from the database. Prior to deletion, the system may aggregate these events into high-level, anonymous historical metrics (e.g. total monthly scans per channel, total unique active cards per month) that contain no event-level details or hashes.
