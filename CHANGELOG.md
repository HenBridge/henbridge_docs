# Changelog

All notable changes to this repository are documented here. Format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Added

- Full documentation set: concept note, data model, threat model, privacy
  design, data retention policy, funding & DPG notes, detailed roadmap,
  personas, FAQ, forward-looking API/contract surface sketch, testing
  strategy, and style guide
- Architecture Decision Records (ADR 0001–0006) covering the Stellar/Soroban
  choice, the on-chain/off-chain boundary, attester governance, the
  `henbridge-web` stack, USDC incentives, and attester key management
- Contributor-facing files: `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`,
  `SECURITY.md`, `LICENSE` (Apache-2.0), PR and issue templates, a
  `CODEOWNERS` placeholder
- Markdown lint CI workflow and configuration
- README restructured to a professional, org-wide format covering overview,
  architecture, trust model, repository structure, roadmap, and cross-repo
  conventions
- README's HenBridge Organization section gained a linked repo table, a
  "Where to Start (per repo)" guide, and expanded AI-agent conventions
- License badge; Roadmap section now links to `docs/roadmap.md` instead of
  duplicating it silently; Tech Stack choices link to their ADRs

### Fixed

- Markdown lint findings across the repo (table style, missing fenced-code
  languages, inconsistent emphasis markers, bold text used as headings, a
  bare URL, and stray blank lines) — CI's markdown-lint workflow now passes
  cleanly
- `docs/funding-and-dpg.md`'s DPG licensing checklist row, which still said
  "planned" after `LICENSE` was added to this repo
- Sibling repo name corrected everywhere: the real Soroban contracts repo is
  `henbridge-contract` (singular), not `henbridge-contracts` — the old name 404'd

### Changed

- `henbridge-web` and `henbridge-contract` marked **in progress** (real code now
  exists: route groups + API + Supabase in `henbridge-web`; attestation-registry
  and attester-registry crates in `henbridge-contract`), and `.github` /
  `henbridge-verifier` marked as **reserved placeholders** (repo exists, README
  only) — Core Components, the org table, "Where to Start", Getting Started,
  and the roadmap all updated to match
- Roadmap's M0 status moved from "not started" to "in progress" in both the
  README and `docs/roadmap.md`
- "Shared Contracts" heading and its anchor shortened now that the sibling
  repos actually exist (dropped the "once they exist" qualifier)
- `LICENSE` switched from Apache-2.0 to **MIT**, matching the license already
  used by `henbridge-web` and `henbridge-contract`; updated the license badge,
  README's License section, and the DPG licensing checklist to match

### Removed

- `AI.md` — its cross-repo map was folded into README's HenBridge Organization
  section instead of living as a separate file
