# HenBridge Concept Note

## Problem statement

In Nigeria, health records are paper-based, siloed per facility, and
effectively lost the moment a patient moves, is referred elsewhere, or
arrives at a facility unconscious. In an emergency, the facts that decide
treatment — genotype (AS/SS sickle-cell status), blood group, and drug
allergies chief among them — are usually unknown to whoever is treating the
patient. Clinicians fall back on assumption or delay treatment to run tests a
portable record would have made unnecessary. Both outcomes cost lives.

Three structural gaps compound this:

1. **No portable record.** A paper card, if one exists, does not survive a
   move, a referral, or an emergency.
2. **No way to trust a claim.** Even when a patient states their blood group
   or allergy, a first responder under time pressure has no way to verify it.
3. **No sustainable last-mile distribution.** Community health workers (CHWs)
   are best positioned to register and verify patients at scale, but have no
   economic incentive to do this work consistently.

## Solution overview

HenBridge is a two-layer system:

- **HenBridge Card** is the patient-facing product: a free, patient-owned
  profile behind a login, exposing a minimal, read-only public emergency page
  reachable by scanning a QR code.
- **HenBridge Proof** is the Stellar-native trust and payment layer underneath
  it: Soroban attestations that let a verified health worker cryptographically
  vouch for a record without ever putting the health data itself on-chain,
  and USDC micropayments that make CHW-driven registration economically
  sustainable.

The result: a patient who cannot speak for themselves still has their
critical facts available, and a responder can trust those facts because a
real, accountable health worker verified them — not because the patient's
phone says so.

## Theory of change

If patients carry a verifiable emergency record, **and** responders can trust
it in seconds without a login or a phone call, **and** CHWs are paid to build
and verify that record at the last mile, **then** emergency treatment
decisions are made on facts instead of assumptions, and fewer patients are
harmed by wrong assumptions about blood group, genotype, or allergy status.

This only works if all three legs hold. A card nobody trusts is theater. A
trust mechanism nobody uses because no one registers patients is unused
infrastructure. An incentive with no verification behind it is just a
subsidy. HenBridge Card, the Soroban attestation registry, and the CHW incentive
rail are designed as one system for this reason — see
[Why Stellar](adr/0001-why-stellar.md) for why none of the three legs can be
cut without the others collapsing.

## Success metrics

Tracked from M0 onward (see [roadmap.md](roadmap.md)):

- Verified emergency cards created (registration + attestation both complete)
- Scan events on public emergency pages (a proxy for real-world reach — see [analytics-spec.md](analytics-spec.md))
- CHW registrations paid out, and time from registration to payout
- Disputed or corrected attestations (a proxy for verification quality)

## Non-goals

- HenBridge is not a medical device and does not make treatment recommendations
  — see the [root README's Disclaimer](../README.md#disclaimer).
- HenBridge does not store full clinical history on the public page — see
  [data-model.md](data-model.md) for exactly what is and isn't exposed.
- HenBridge does not put any personal health data on-chain, ever — see
  [privacy-design.md](privacy-design.md).
