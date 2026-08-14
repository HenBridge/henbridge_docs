# Funding & Digital Public Goods Notes

HenBridge is built as an open-source Digital Public Good targeting SDG 3 (Good
Health and Well-being). See the root README's
[Why This Matters](../README.md#why-this-matters) section for the funding
sequence (SCF → DPG registration → DPG-aligned funders).

## Stellar Community Fund (SCF) — Build track

Positioning notes for the application:

- HenBridge's use of Stellar is load-bearing, not decorative — the attestation
  registry and CHW incentive rail both depend on Soroban and Stellar-native
  USDC; see [ADR 0001](adr/0001-why-stellar.md).
- M0–M2 (see [roadmap.md](roadmap.md)) map directly to the Build track's
  expectation of a working testnet product before mainnet funding.

## Digital Public Goods Standard checklist

| DPG Standard requirement | Status |
| --- | --- |
| Relevance to SDGs | Met — SDG 3, Good Health and Well-being |
| Open licensing | Done — MIT across all three active repos (`henbridge-docs`, `henbridge-web`, `henbridge-contract`); see [LICENSE](../LICENSE) |
| Clear ownership | Held under the `henbridge` GitHub organization |
| Platform independence | Met by design — no proprietary lock-in; Next.js, Postgres, Soroban are all portable |
| Documentation | This repo |
| Mechanism for extracting data | To be implemented in `henbridge-web` (patient data export) |
| Adherence to privacy & applicable laws | See [privacy-design.md](privacy-design.md) and the NDPA 2023 mapping |
| Do no harm — data privacy & security | See [threat-model.md](threat-model.md) |

This checklist will be revisited before formal DPG registration; items marked
"Planned" or "To be implemented" are tracked against the
[roadmap](roadmap.md).
