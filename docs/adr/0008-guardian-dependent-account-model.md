# ADR 0008: Guardian/dependent account model

## Status

Proposed

## Context

The current [data model](../data-model.md) assumes one profile = one adult
patient, but the primary patient persona is often "a mother registering a
child" (see [personas.md](../personas.md)) — this gap is tracked as a schema
issue in the Data Model category (`[Data Model] Add support for
guardian/dependent profiles to the data model`, issue #8) and has a
consent-lifecycle counterpart in the Privacy & Legal category (`Design a
consent lifecycle for minors reaching age of majority`, issue #26). This ADR
is the design decision underlying both: how the guardian/dependent
relationship is represented, how consent works while a dependent is a minor,
and what happens at the age of majority. Issue #8's schema work and issue
#26's lifecycle work should follow the decision recorded here rather than
each inventing its own model.

## Decision

**Relationship representation.** A dependent's profile carries an
`account_type: independent | dependent` field. Guardianship is a many-to-many
`guardian_relationship` table (`dependent_patient_id`, `guardian_patient_id`,
`relationship_type`, `established_at`, `revoked_at`), not a single foreign
key — this supports more than one guardian per dependent (e.g. both parents)
and one guardian across multiple dependents (siblings). A guardian must
themselves hold an authenticated Lafiya account; guardianship is
self-asserted at registration (consistent with CHW-driven, low-friction
registration in the field) rather than gated behind upfront legal-document
verification, with disputes handled through governance after the fact — the
same accountability-not-prevention pattern already used for attester
integrity (see [ADR 0003](0003-attester-allowlist-governance.md)'s
consequences).

**Consent handling while dependent.** The guardian is the sole consent
authority for a dependent's profile: editing the full profile and toggling
[emergency-subset](../data-model.md#emergency-subset-public-qr-reachable)
visibility. Every guardian-made consent or edit action is attributed to the
acting `guardian_patient_id`, not silently merged into the dependent's own
history, so the audit/non-repudiation property [threat-model.md](../threat-model.md)
relies on for every other actor holds for guardians too. Any single guardian
on the relationship may make changes; all guardians on the relationship can
see the change history. Resolving disagreement between co-guardians (e.g. a
custody dispute) is out of scope for this ADR and is left to the governance
dispute process, the same way a coerced attester is ([ADR 0003](0003-attester-allowlist-governance.md)).
The emergency subset's *shape* is unchanged for dependents — the same fields
apply — only who controls its consent flags differs.

**Age-of-majority transition.** At age 18 (the age of majority under
Nigeria's Child's Rights Act, consistent with NDPA 2023's scope), a
dependent transitions on a grace-period model rather than a hard cutover:

- Guardian write access is revoked automatically at majority.
- Existing consent flags carry over unchanged for a 90-day grace window,
  so the emergency page does not go dark the moment someone turns 18 —
  preserving the core safety guarantee is more important than requiring
  instant re-consent.
- The now-adult patient is notified and prompted to explicitly reaffirm
  their consent settings on next login within the grace window.
- Any field not reaffirmed by the end of the grace window reverts to the
  platform's documented safe defaults ([privacy-design.md](../privacy-design.md#data-minimization--consent):
  blood group and genotype on, everything else off) rather than staying on
  the guardian's prior choice indefinitely — this bounds how long a
  guardian's consent can stand in for the data subject's own.

This ADR is left "Proposed": issue #8 should model the schema fields
described here, and issue #26 should specify the notification/reaffirmation
UX in detail; both should reference this decision rather than diverge from
it.

## Consequences

- Adds `account_type` and a `guardian_relationship` table to the shared
  data-model contract ([data-model.md](../data-model.md#versioning)),
  which `lafiya-web`'s schema must reflect in the same or a tracked
  follow-up change.
- Requires a scheduled process to detect dependents crossing the
  majority-age-plus-grace-window boundary and revert unreviewed consent
  flags to safe defaults — a new operational component, not just a schema
  change.
- Self-asserted guardianship (no upfront legal verification) makes
  onboarding fast but means a false guardian claim is a real, if bounded,
  risk — mitigated only by governance dispute-handling after the fact, the
  same residual-trust tradeoff already accepted for attesters
  ([ADR 0003](0003-attester-allowlist-governance.md)).
- Directly affects account recovery: while dependent, the guardian is the
  recovery path per [ADR 0007](0007-patient-account-recovery.md), so this
  model must exist before that ADR's guardian-recovery tier can be
  implemented.
- The grace-period default-reversion keeps a lapsed transition from ever
  fully blacking out the emergency page (safe defaults stay on), trading a
  small amount of over-retained guardian-set consent for continuity of the
  platform's core safety guarantee.
