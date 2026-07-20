# Lafiya Bug Bounty Program Specification

## Overview

Lafiya is a **Stellar-based decentralized platform for health data** operating on **testnet** pre-mainnet. This bug bounty program formally defines the scope, reward structure, and disclosure policy for security researchers contributing to the platform's security.

Given the **pre-mainnet, constrained-budget** stage, rewards are structured as **Stellar testnet credits** (convertible to mainnet XLM post-launch) plus **reputation recognition** in the project's governance forum.

---

## 1. In-Scope Targets

### 1.1 In-Scope Repositories

| Repository | Priority | Description |
|---|---|---|
| `lafiya-contract` | **Critical** | Stellar smart contracts (DPG - Data Protection Gateway) |
| `lafiya-web` | **High** | Frontend web application |
| `lafiya-docs` | **Medium** | Design docs, threat model, privacy design (design-level flaws) |

### 1.2 In-Scope Vulnerability Types

- **Smart contract bugs**: reentrancy, authorization bypass, asset theft, logic errors in attestation scheme
- **On-chain/off-chain boundary flaws**: described in [privacy-design.md](privacy-design.md)
- **Data model privacy issues**: fields in the public emergency subset that shouldn't be exposed ([data-model.md](data-model.md))
- **Threat model gaps**: missing attack vectors in [threat-model.md](threat-model.md)
- **Access control failures**: improper permissions in the wallet/SAC integration
- **Frontend security**: XSS, CSRF, insecure direct object references

### 1.3 Out-of-Scope Targets

- DoS, brute force, social engineering attacks
- Issues in third-party dependencies (file separately with the upstream project)
- Cosmetic UI/UX bugs (create a GitHub Issue instead)
- Testnet infrastructure issues not related to Lafiya code

---

## 2. Severity-Based Reward Tiers

### 2.1 Reward Structure

Rewards are denominated in **Stellar testnet credits (XLM)**. Actual mainnet value determined post-launch based on XLM market rate. All rewards are paid to the researcher's **Stellar wallet address** upon successful fix merge and verification.

| Severity | Testnet Reward | Mainnet Equivalent (USD est.) | Description |
|---|---|---|---|
| **Critical** | 5,000 XLM | ~$1,500 | Direct asset theft, wallet drain, bypass of attestation scheme, full account takeover |
| **High** | 2,000 XLM | ~$600 | Partial data leak of private health data, contract logic bypass, privilege escalation |
| **Medium** | 500 XLM | ~$150 | Design flaw in privacy model, missing access control, information disclosure |
| **Low** | 100 XLM | ~$30 | Minor threat model gap, documentation security note, missing input validation |
| **Informational** | 50 XLM | ~$15 | Valid finding with no practical exploitation path but improves security posture |

### 2.2 Bonus Tiers

- **First report bonus**: +25% of base reward for first valid report of any type
- **Multiple related findings**: +10% per additional related finding in same report (max +30%)
- **Proof-of-concept**: +20% if researcher provides working PoC
- **Patch provided**: +15% if researcher provides a working fix with PR

### 2.3 Non-Financial Recognition

- **Hall of Fame**: Top 5 researchers per quarter listed in project governance forum
- **Governance participation**: Critical/High reporters invited to security committee
- **Reputation score**: On-chain reputation NFT minted for verified reporters

---

## 3. Disclosure Timeline

### 3.1 Private Disclosure Period

- **Initial report**: Security Advisory via GitHub (private)
- **Triage deadline**: 5 business days for project response
- **Private disclosure period**: **90 days** from initial report

### 3.2 Timeline Stages

| Stage | Time from Report | Action |
|---|---|---|
| **Triage** | Day 0–5 | Project confirms receipt, assigns severity, estimates fix timeline |
| **Fix development** | Day 5–30 | Project develops and tests fix (complexity dependent) |
| **Researcher verification** | Day 30–60 | Researcher confirms fix, may provide additional feedback |
| **Pre-release coordination** | Day 60–90 | Public advisory draft, coordinated disclosure planning |
| **Public disclosure** | Day 90 | Public advisory + fix release simultaneously |

### 3.3 Exceptions

- **Critical vulnerabilities** (asset theft, account takeover): Disclosure period reduced to **60 days**
- **Researcher request**: Researcher may request early public disclosure (no earlier than 45 days)
- **Project emergency**: Project may request extended disclosure period (up to 120 days) with researcher consent
- **Legal requirement**: If law requires immediate disclosure, both parties notify each other 48h before

### 3.4 Responsible Disclosure Rules

- Researcher agrees not to publicly disclose the vulnerability during the private period
- Researcher agrees not to exploit the vulnerability against live systems
- Project agrees to provide status updates every 14 days during the private period
- Both parties agree to jointly draft the public advisory

---

## 4. How to Report

### 4.1 Reporting Channels

1. **Preferred**: [GitHub Security Advisory](https://github.com/Lafiya-xyz/lafiya-docs/security/advisories/new) — private, tracked, auditable
2. **Alternative**: Email `security@lafiya.xyz` (PGP key available on project website)

### 4.2 Report Requirements

- Clear description of the vulnerability
- Steps to reproduce (with screenshots/PoC where possible)
- Affected repository and component
- Impact assessment
- Suggested fix (optional but rewarded)

### 4.3 What NOT to Do

- ❌ Create public GitHub Issues for security vulnerabilities
- ❌ Share the finding with anyone other than the project team
- ❌ Attempt to exploit on testnet beyond proof-of-concept
- ❌ Disclose before the agreed timeline

---

## 5. Program Governance

### 5.1 Eligibility

- Open to any individual or team (no geographic restrictions)
- No prior experience required — first-time reporters welcome
- No requirement to use pseudonym

### 5.2 Program Review

- Quarterly review of reward tiers and scope
- Bounty amounts adjusted for inflation (Stellar XLM price) and budget availability
- Program maintained in project governance forum

### 5.3 Exclusions

- Employees and contractors of the Lafiya team are ineligible
- Researchers who have previously received payment for the same issue are ineligible for duplicate rewards

---

## 6. Contact

- **Security reports**: GitHub Security Advisory (preferred) or `security@lafiya.xyz`
- **Program questions**: `security@lafiya.xyz`
- **Governance forum**: [Lafiya Forum](https://forum.lafiya.xyz)
- **PGP key**: Available at `https://lafiya.xyz/security/pgp-key.txt`

---

_Last updated: 2026-07-10_
