# HenBridge — Docs 🩺

[![Built on Stellar](https://img.shields.io/badge/Built%20on-Stellar-blue?logo=stellar)](https://stellar.org)
[![Soroban](https://img.shields.io/badge/Trust%20layer-Soroban-purple)](https://soroban.stellar.org)
[![Status: Pre-alpha](https://img.shields.io/badge/Status-Pre--alpha-orange)](#status)
[![License: MIT](https://img.shields.io/badge/License-MIT-informational)](LICENSE)

**The specification the code obeys.**

This is the design repository for HenBridge — a free, patient-owned emergency health card on Stellar. It holds no application code; it holds the decisions the code is built against: the concept note, the emergency data model, the threat and privacy models, the funding/DPG materials, and the architecture decision records. When the apps and the docs disagree, that's a bug — and usually the doc is the one that's right.

<a id="status"></a>
> **Where things stand:** Pre-alpha · Stellar **testnet** · not audited · **not a medical device** (see [Disclaimer](#disclaimer)).

---

## The product in a paragraph

In Nigeria, the facts that decide emergency treatment — **genotype (AS/SS sickle-cell status), blood group, drug allergies** — are usually unknown to whoever is treating a patient who has moved, been referred, or arrived unconscious. HenBridge gives each patient a QR-reachable emergency page carrying just those facts, working offline and no-login for the responder, and marked *verified* when an allowlisted health worker has attested it on Stellar. The health data never touches the chain; only hashes, attestations, and USDC payments do. Community health workers get paid per verified registration, which is what makes the last mile actually get covered.

The reason Stellar is load-bearing, stated once: it makes a verification **tamper-evident and independently checkable without exposing the data**, and it settles **cross-border stablecoin micropayments** for sub-cent fees. Take it away and both the trust layer and the incentive engine disappear.

## The canonical emergency data model

Everything downstream mirrors this list — the frontend's profile schema, the backend's field validation, the contract's record hash. It is deliberately minimal: only what changes treatment in the first minutes.

- Name, age, photo
- **Blood group and genotype**
- Drug allergies
- Current medications (esp. anticoagulants, insulin, anti-epileptics)
- Chronic conditions / implants
- Emergency contact(s)
- Spoken language

Full history, documents, and notes stay private, behind authentication.

## The trust model, in five terms

| Term | Meaning |
| --- | --- |
| **On-chain attestation** | An allowlisted worker verifies a record; Soroban stores `hash(record) + attester identity + timestamp` |
| **Off-chain data** | The full record lives encrypted in Postgres under row-level security; it never touches the chain |
| **Verified indicator** | A responder's scan checks the registry for a matching hash and shows a *verified* badge |
| **Attester allowlist** | Only health workers approved through governance can write attestations |
| **Incentive rails** | CHWs are paid micro-amounts of USDC on Stellar per verified registration |

> **The invariant everything hangs on:** no personal health data is ever written on-chain. It is what keeps HenBridge private *and* regulator-compatible, and it is a hard rule, not a preference.

## Read the docs

Everything lives in [`docs/`](docs/README.md) — start with the index there. By theme:

**Concept & product**
[Concept note](docs/concept-note.md) · [Personas](docs/personas.md) · [FAQ](docs/faq.md) · [Glossary](docs/glossary.md) · [Roadmap](docs/roadmap.md)

**Data, privacy & security**
[Data model](docs/data-model.md) · [Threat model](docs/threat-model.md) · [Privacy design](docs/privacy-design.md) · [Data retention policy](docs/data-retention-policy.md)

**Architecture decisions** — [`docs/adr/`](docs/adr/README.md)
0001 Why Stellar · 0002 Off-chain data, on-chain attestation · 0003 Attester allowlist governance · 0004 Next.js + Supabase stack · 0005 USDC for CHW incentives · 0006 Verification key management · 0007 Patient account recovery · 0008 Guardian / dependent account model

**Operations & assurance** *(some forward-looking)*
[API & contract surface sketch](docs/api-surface-sketch.md) · [Testing strategy](docs/testing-strategy.md) · [Analytics spec](docs/analytics-spec.md) · [Audit-readiness checklist](docs/audit-readiness-checklist.md) · [Dependency risk](docs/dependency-risk.md) · [Certification roadmap](docs/certification-roadmap.md) · [Bug bounty](docs/bug-bounty.md)

**Funding & governance**
[Funding & DPG notes](docs/funding-and-dpg.md) · [Style guide](docs/style-guide.md)

Report a design-level security or privacy concern via [SECURITY.md](SECURITY.md).

## Running the product

Nothing runs here — this repo is documentation. To run HenBridge, start with the app:

```bash
git clone https://github.com/HenBridge/henbridge_frontend
cd henbridge_frontend
cp .env.example .env.local   # Supabase + Stellar testnet keys
npm install && npm run dev
```

## Milestones

Full definitions of "done" per milestone are in [docs/roadmap.md](docs/roadmap.md).

- **Phase 0 — Docs foundation.** *Done.* Concept, data/threat/privacy models, ADRs, contributor infra.
- **M0 — Public card (testnet).** Profile, public emergency page, QR, offline. *In active development in `henbridge_frontend`.*
- **M1 — Attestation.** Allowlisted attester verifies a record; the card shows a verified indicator. *Contracts implemented in `henbridge_contract`.*
- **M2 — Incentives.** USDC-on-Stellar payout per verified registration.
- **M3 — Pilot.** Supervised field pilot; measure verified cards and scans.
- **M4 — Mainnet + funding.** Mainnet launch; open the transparent funding pool.

HenBridge is built as an open-source **Digital Public Good** (SDG 3, Good Health and Well-being): primary route is the **Stellar Community Fund (Build track)**, bridged by registration against the **Digital Public Goods Standard**.

## The HenBridge org

This repo is the spec; three code repos build against it. A change to a shared contract (below) is a change everywhere that consumes it.

| Repo | What it holds |
| --- | --- |
| [`henbridge_frontend`](https://github.com/HenBridge/henbridge_frontend) | Patient + responder web app — public card, profile editor, QR, offline |
| [`henbridge_backend`](https://github.com/HenBridge/henbridge_backend) | CHW service — register records, submit attestations, queue USDC payouts |
| [`henbridge_contract`](https://github.com/HenBridge/henbridge_contract) | Soroban (Rust): attester allowlist + attestation registry + multisig |
| **`henbridge_docs`** *(this repo)* | Concept, data model, threat model, privacy design, ADRs |

```
henbridge_docs ─(data model · threat model · privacy design)─▶ henbridge_frontend
                                                                      │  patient profile + QR
   CHW verifies a record ─▶ henbridge_backend ─(hash + attester + timestamp)─▶ henbridge_contract
                                                                      │
                                                     verified flag on the public card
                                                                      │
                                              responder scans, trusts · USDC payout to the CHW
```

### Shared contracts (keep in sync)

- **Emergency data model** — the field list above is the source of truth; `henbridge_frontend`'s profile schema and `henbridge_backend`'s validation mirror it field-for-field.
- **Attestation shape** — `record_hash` + `attester identity` + `timestamp`; `henbridge_contract`'s registry must match. Check for drift whenever either side moves.
- **Config keys** — the Supabase and Stellar keys are defined in `henbridge_frontend`'s `.env.example`.

### Working conventions

- This section is the source of truth for *cross-repo* contracts; each code repo's README owns its local conventions.
- This repo is **documentation only** — no application or contract code, and never any personal health data, secrets, or private keys.
- A change here that touches a shared contract must be flagged with a matching issue in the affected repo(s) — don't let the specs and the code drift silently.
- Don't invent URLs, contacts, or people; leave a marked placeholder rather than a fabricated fact.

## Tech stack (as decided in the ADRs)

- **App:** Next.js on Vercel — [ADR 0004](docs/adr/0004-nextjs-supabase-stack.md)
- **Data & auth:** Supabase — Postgres, Row-Level Security, encryption at rest — [ADR 0004](docs/adr/0004-nextjs-supabase-stack.md)
- **On-chain:** Soroban (Rust) on Stellar; USDC for incentives — [ADR 0001](docs/adr/0001-why-stellar.md), [ADR 0005](docs/adr/0005-usdc-for-chw-incentives.md)
- **Standards:** W3C Verifiable Credentials; HL7 FHIR for field structure

## Contributing

This repo is prose, so most contributions are proposals or corrections to the docs above. See [CONTRIBUTING.md](CONTRIBUTING.md) and [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md).

## License

**MIT** — see [LICENSE](LICENSE). Matches `henbridge_frontend` and `henbridge_contract`; OSI-approved and satisfies the Digital Public Goods Standard's open-licensing requirement.

## Disclaimer

HenBridge is an information aid — **not a medical device**, not a substitute for professional judgment. A verified indicator means a registered health worker attested the record; it is not a clinical guarantee. The attending clinician owns the treatment decision.

---

<div align="center">

**HenBridge** — the decisions the code is built on.

_Built on Stellar · open source · community-owned._

</div>
