## Summary

## Type of change

- [ ] Documentation fix/clarification
- [ ] New documentation (concept note, data model, threat model, privacy design, ADR, etc.)
- [ ] Process/template change (issue templates, CI, contributing guide)
- [ ] Other (describe above)

## Shared-contract impact

- [ ] This PR changes the emergency data model, attestation schema, or another
      contract shared with `henbridge-web` / `henbridge-contracts` — the matching
      issue in the affected repo is linked below.

### Shared-contract review checklist

_Complete this if the box above is checked. Changes to shared contracts can
silently break `henbridge-web` or `henbridge-contract` — this enforces the
"Shared Contracts" rule from the README and CONTRIBUTING.md._

- [ ] I confirmed whether this PR touches any **Shared Contracts** section item
      (emergency subset fields, full-profile fields, attestation record shape,
      attester allowlist, env/config keys) — see
      [data model](docs/data-model.md) and the
      [README Shared Contracts section](../../README.md#shared-contracts-must-stay-in-sync-across-repos).
- [ ] If a **data-model or attestation-schema** field changed: the matching
      update to `henbridge-web`'s schema and/or `henbridge-contract`'s Rust struct is
      in this change set **or** a tracked follow-up issue is linked below.
- [ ] If an **API surface** field changed: the backend data-contract update in
      the affected repo is linked (cross-repo follow-up issue required).
- [ ] I added/updated an ADR if the shared-contract change is a design
      decision (see [adr/](adr/README.md)).
- [ ] I noted the cross-repo impact in the PR description so reviewers in the
      other repo are alerted.

## Checklist

- [ ] Markdown lint passes (`npx markdownlint-cli2 "**/*.md"`)
- [ ] Links resolve and any table of contents matches the headings
- [ ] I read [CONTRIBUTING.md](../CONTRIBUTING.md) and [CODE_OF_CONDUCT.md](../CODE_OF_CONDUCT.md)
