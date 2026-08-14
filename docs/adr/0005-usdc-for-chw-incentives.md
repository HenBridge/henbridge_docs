# ADR 0005: USDC on Stellar for CHW incentives

## Status

Accepted

## Context

CHW payments need to be small (per-registration micropayments), frequent,
and low-fee, including potentially cross-border, without exposing CHWs to
price volatility.

## Decision

Pay CHWs in USDC on Stellar rather than XLM or a fiat rail.

## Consequences

- CHWs receive a stable-value payout, not a volatile asset.
- Stellar's low fees make frequent micropayments viable in a way a
  card-network or bank-transfer rail would not be.
- CHWs need a Stellar wallet capable of holding USDC — an onboarding step
  that must be designed for people who may not have prior crypto experience
  (tracked as an open design question for `henbridge-verifier`).
