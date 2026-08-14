# ADR 0004: Next.js + Supabase for `henbridge-web`

## Status

Accepted

## Context

`henbridge-web` needs authenticated patient profiles with fine-grained
per-field access control, a fast public page for responders, and a small
team able to ship M0 quickly.

## Decision

Next.js (deployed on Vercel) for the app; Supabase (Postgres + Row-Level
Security + encryption at rest) for data and auth.

## Consequences

- Row-Level Security maps naturally onto the emergency-subset vs.
  full-profile split in [data-model.md](../data-model.md).
- Vercel + Supabase is a fast path to a working M0 without standing up
  custom infrastructure.
- Both are hosted third-party services — acceptable for pre-alpha/testnet,
  but [data-retention-policy.md](../data-retention-policy.md) and backup
  handling must account for a vendor-hosted database.
