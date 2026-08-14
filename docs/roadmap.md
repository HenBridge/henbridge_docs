# Roadmap (detail)

This expands the milestone list in the
[root README](../README.md#roadmap). Each milestone is a working increment,
not a sprint — M(n+1) does not start in earnest until M(n) has a working
demo.

## Phase 0 — Documentation foundation

**Goal:** every design decision M0 depends on is written down before any app
or contract code exists. Status: **done**, entirely within this repo.

- [x] Concept note, data model, threat model, and privacy design specified
- [x] Architecture Decision Records for the Stellar/Soroban trust model, the
      `henbridge-web` stack, and CHW incentives (see [adr/](adr/README.md))
- [x] Contributor infrastructure: `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`,
      `SECURITY.md`, `LICENSE` (MIT), issue/PR templates, and
      markdown-lint CI
- [x] `henbridge-web` and `henbridge-contract` repos scaffolded and under active
      development

## M0 — Public card (testnet)

**Goal:** one patient can create a profile and expose a working read-only
emergency page via QR. **Status: in progress** — `henbridge-web` has
authenticated and public route groups, an API layer, and a Supabase
integration; `henbridge-contract` has separate attestation-registry and
attester-registry crates. Not yet verified end-to-end.

- Patient signup/login (`henbridge-web`)
- Profile editor covering the full [data model](data-model.md)
- Public emergency page rendering the emergency subset only
- QR generation pointing at the public page
- No attestation yet — page shows "unverified"

## M1 — Attestation

**Goal:** an allowlisted attester can verify a record; the card shows a
verified indicator.

- Soroban attestation registry contract (`henbridge-contract`), Testnet
- Attester allowlist (governance-managed)
- `henbridge-web` submits the record hash on attestation and checks it on page
  load

## M2 — Incentives

**Goal:** CHWs are paid in USDC per verified registration.

- USDC-on-Stellar payout flow tied to a completed attestation
- CHW-facing view of registrations and payouts (may live in
  `henbridge-verifier`)

## M3 — Pilot

**Goal:** a small, supervised field pilot.

- Metrics: verified cards created, scan events (see
  [concept-note.md](concept-note.md#success-metrics))
- Dispute process exercised at least once end-to-end

## M4 — Mainnet + funding

**Goal:** launch on mainnet; open a transparent funding pool.

- Mainnet deployment of `henbridge-contract`
- Public, on-chain-traceable funding pool for CHW incentives
- DPG Standard registration (see [funding-and-dpg.md](funding-and-dpg.md))

## Security testing cadence

Ongoing penetration testing tied to the roadmap, complementing the one-time
pre-mainnet audit (see the [audit-readiness checklist](docs/audit-readiness-checklist.md)):

- **Scope per milestone:** M1 (attestation registry + allowlist logic),
  M3 (pilot field surface), M4 (full mainnet attack surface).
- **Cadence:** a dedicated pen-test immediately before M3 (pilot) and before
  M4 (mainnet); annually post-mainnet, or on any shared-contract change to
  the [data model](docs/data-model.md) or attestation schema.
- **Vendor-selection criteria:** demonstrated Stellar/Soroban and health-data
  (PHI) engagement experience; able to sign a data-handling agreement;
  delivers a remediated-report cycle, not a one-shot scan.
- See also the [bug bounty program](docs/bug-bounty.md) for continuous
  researcher-driven disclosure.
