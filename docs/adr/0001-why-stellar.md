# ADR 0001: Why Stellar / Soroban

## Status

Accepted

## Context

HenBridge needs two things a plain web app cannot provide on its own: a way for
a responder who has never met the patient to trust a verification claim, and
a way to pay CHWs small, frequent amounts without fees eating the payment.

## Decision

Use Soroban (Stellar's smart contract platform) for the attestation
registry, and Stellar-native USDC for CHW payouts.

## Consequences

- Verification becomes tamper-evident and independently checkable by anyone,
  without a central authority a responder has to trust blindly.
- Micropayments to CHWs become economically viable at near-zero fees,
  including cross-border.
- HenBridge takes on the operational overhead of a testnet/mainnet deployment,
  key management for the service account, and Soroban-specific tooling.
- If Stellar were removed, both the trust layer and the incentive engine
  would need to be rebuilt from scratch on different infrastructure — this
  is treated as a core, load-bearing dependency, not a swappable detail. See
  [privacy-design.md](../privacy-design.md#on-chain--off-chain-boundary).
