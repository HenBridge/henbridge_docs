# ADR 0007: Patient account-recovery mechanism

## Status

Proposed

## Context

[threat-model.md](../threat-model.md#residual-risk) flags "lost patient
credentials" as an open design question with no owning ADR. Because the
public emergency page's usefulness depends on the patient (or guardian)
being able to keep their profile current, losing all access to an account —
not just forgetting a password, but losing every recovery channel at once —
needs a defined, accountable path back in. This is a core UX/security
tradeoff: too weak a recovery process is a spoofing vector (see
[threat-model.md](../threat-model.md), Spoofing); too strict a process
leaves patients locked out of the exact system meant to help them in an
emergency.

Two approaches were considered beyond routine password reset (which already
covers "forgot my password" and is not the gap this ADR addresses):

1. **Recovery contacts** — the patient designates trusted people (M-of-N
   must approve) who can vouch for a recovery request. This decentralizes
   trust to people the patient already knows, requires no new central
   authority, and mirrors patterns users already understand from other
   consumer apps. Its weakness: it depends entirely on the patient having
   proactively set contacts up before losing access, and a colluding or
   coerced set of contacts can take over the account undetected.
2. **Governance-mediated recovery** — the same committee that manages the
   attester allowlist ([ADR 0003](0003-attester-allowlist-governance.md))
   manually verifies the patient's identity out-of-band and authorizes a
   reset. This is accountable and auditable, and needs no advance setup by
   the patient, but it is slow, does not scale with patient volume the way
   CHW-driven registration is meant to, and puts a sensitive, high-trust
   task on a committee whose current mandate is attester governance, not
   patient identity verification.

Neither option alone is sufficient: recovery contacts fail for a patient who
never configured them (very plausible for CHW-registered patients — see
[personas.md](../personas.md)), and governance-mediated recovery alone
doesn't scale as the only path for every lockout.

## Decision

Layer both mechanisms rather than choosing one:

1. **Recovery contacts (primary)** — at profile setup, a patient may
   designate recovery contacts, distinct from the plain-text
   `emergency_contacts` field in the [data
   model](../data-model.md#full-patient-profile-private-authenticated) (a
   recovery contact must itself be an authenticated Lafiya account able to
   approve a request; `emergency_contacts` is unauthenticated free text
   shown to responders and is not fit for this purpose). Recovery requires
   approval from a majority of configured recovery contacts.
2. **Guardian as recovery path for dependents** — where a patient is a
   dependent under [ADR 0008](0008-guardian-dependent-account-model.md),
   the guardian *is* the recovery path and a separate recovery-contacts
   setup is not required.
3. **Governance-mediated recovery (fallback)** — if no recovery contacts
   were configured, or they are unreachable/unresponsive, the patient can
   request recovery through the same governance/committee process used for
   attester-allowlist decisions ([ADR 0003](0003-attester-allowlist-governance.md)),
   using out-of-band identity verification. This is the backstop of last
   resort, not the default path, to keep committee load bounded.

This ADR is left "Proposed": the specific identity-verification steps for
the governance fallback should be finalized alongside the committee's
operational process, not invented here in the abstract.

## Consequences

- Introduces a new `recovery_contacts` relation, distinct from
  `emergency_contacts`, which is a shared-contract change per
  [data-model.md](../data-model.md#versioning) and needs to be reflected in
  `lafiya-web`'s schema.
- Gives the governance committee a second responsibility beyond the
  attester allowlist; committee load and process design should account for
  both when [ADR 0006](0006-verification-key-management.md)'s key-custody
  model and this recovery process are finalized together.
- A patient who configures no recovery contacts and cannot reach governance
  quickly is locked out for longer — an explicit tradeoff against making
  governance the mandatory path for everyone, which would not scale.
- Recovery-contact collusion or coercion is a residual spoofing risk (see
  [threat-model.md](../threat-model.md), Spoofing); M-of-N approval reduces
  but does not eliminate it, and any successful account takeover is still
  bounded by the same tamper-evidence property that protects every profile
  edit — an edit invalidates the existing attestation hash, so a hijacked
  account cannot silently produce falsified *verified* data.
- Directly informs the guardian/dependent model in
  [ADR 0008](0008-guardian-dependent-account-model.md), since guardian
  status changes who the recovery path even is.
