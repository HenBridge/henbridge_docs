# Testing Strategy (forward-looking)

> As with [api-surface-sketch.md](api-surface-sketch.md), this describes how
> the eventual code should be tested — there is no code in this repo to test
> yet.

## `henbridge-web`

- Unit tests for visibility/consent logic (a field marked private must never
  appear in the public emergency page response)
- Integration tests for the public emergency page against the full
  [data model](data-model.md)
- Contract tests against `henbridge-contract`'s testnet deployment for the
  attestation flow

## `henbridge-contract`

- Unit tests for `attest`, `get_attestation`, `add_attester`,
  `remove_attester`
- Explicit negative tests: a non-allowlisted address must never succeed in
  calling `attest`
- Fuzz/property tests on hash handling — the contract must never accept or
  emit anything other than an opaque `BytesN<32>`

## Cross-repo

- A shared fixture set (patient records + expected hashes) so `henbridge-web`
  and `henbridge-contract` tests exercise the same data model, kept here in
  `henbridge-docs` once fixtures are formalized.
- Any test asserting privacy behavior (private field never leaks) is treated
  as security-critical and should block merges on failure, not just warn.
