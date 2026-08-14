# ADR 0006: Attester key management

## Status

Proposed

## Context

An attester's Stellar keypair is what makes their attestation meaningful —
if it leaks, an attacker can forge verified records under that health
worker's identity (see [threat-model.md](../threat-model.md), Spoofing).

## Decision (proposed)

Attester keys are managed per health worker (not a single shared service
key), with key custody options to be finalized before M1 — candidates
include a managed custodial wallet issued at allowlist onboarding, or a
self-custodied keypair with a recovery process run by governance.

## Consequences

- Per-attester keys make revocation surgical (remove one attester from the
  allowlist without affecting others) rather than an all-or-nothing
  rotation.
- Key custody UX is now a hard dependency for M1 — a health worker who
  can't manage a keypair reliably can't attest, which would stall the entire
  trust layer.
- This ADR is intentionally left "Proposed": the final custody model should
  be decided alongside `henbridge-contract`'s allowlist implementation, not
  before.
