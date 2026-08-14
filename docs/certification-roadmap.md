# Security Certification & Compliance Roadmap

Assesses which formal certifications are applicable to HenBridge at its current
pre-alpha/pilot stage, and drafts a **staged** path toward them — pursuing
each only when actually warranted by scale, funder requirement, or regulatory
exposure. HenBridge is a health-data Digital Public Good, so this is revisited
before formal DPG registration (see
[funding-and-dpg.md](funding-and-dpg.md)).

## Certifications researched

### SOC 2 Type II

- **What:** audited controls for security, availability, and
  confidentiality of a hosted service.
- **Applicability now:** **Not yet warranted.** Pre-mainnet, testnet-only,
  no real PHI. The control environment (incident response, access reviews,
  vendor management) is not mature enough to sustain a 12-month audit
  window.
- **When it becomes relevant:** at M4 (mainnet + funding) if a funder or
  institutional partner requires it, or if real patient data is processed.

### ISO 27001

- **What:** international ISMS standard; certifiable.
- **Applicability now:** **Not yet warranted.** Heavy documentation and
  annual surveillance overhead disproportionate to a 3-repo pilot.
- **When it becomes relevant:** if HenBridge scales to multiple deployment
  regions or joins a health-system procurement framework that mandates it.

### DPG Standard (already tracked)

- **What:** the [Digital Public Goods Standard](funding-and-dpg.md) — open
  source, open data, open AI, content, and community.
- **Applicability now:** **Actively pursued.** This is the right-stage
  certification for a pre-mainnet DPG and aligns with the SCF Build track.
- **Status:** checklist maintained in `funding-and-dpg.md`.

### HIPAA / NDPA alignment

- **What:** US (HIPAA) and Nigeria (NDPA 2023) health-data law.
- **Applicability now:** NDPA 2023 mapping is **in progress** (see
  [privacy-design.md](../docs/privacy-design.md)); full HIPAA is out of scope
  until a US deployment exists.
- **When it becomes relevant:** if HenBridge serves US-resident data.

## Staged roadmap

| Stage | Trigger | Action |
| --- | --- | --- |
| Pre-alpha (now) | Testnet only, no real PHI | DPG Standard checklist; NDPA 2023 mapping; document the [bug bounty](../docs/bug-bounty.md) and [secure-SDLC](../CONTRIBUTING.md#secure-sdlc-policy) |
| M3 (pilot) | Supervised field pilot, synthetic/seeded data | Internal security review; formalize incident-response runbook |
| M4 (mainnet + funding) | Real PHI possible | Re-assess SOC 2 / ISO 27001 against funder requirements; budget the audit |
| Scale | Multi-region / institutional partner | Commit to the certifications the partner mandates; assign an owner |

## Cost / effort tradeoffs

- DPG Standard: low effort, high signal for funders — **do it**.
- SOC 2 Type II: high effort (12-month window + audit fee) — defer until a
  funder requires it; premature certification wastes constrained budget.
- ISO 27001: highest overhead — defer until procurement demands it.
- The guiding principle: **certify for a real requirement, not for optics.**
