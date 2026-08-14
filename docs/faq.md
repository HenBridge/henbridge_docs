# FAQ

**Is my health data stored on the blockchain?**
No. Only a hash, the attester's identity, and a timestamp are ever written
on-chain. See [privacy-design.md](privacy-design.md).

**What can a responder see when they scan my QR code?**
Only the emergency subset you've chosen to make public — see
[data-model.md](data-model.md#emergency-subset-public-qr-reachable). They
cannot see your full profile, and they don't need a login.

**What does "verified" mean on my card?**
It means a registered, allowlisted health worker attested on-chain that your
record matches what's shown. It is not a clinical guarantee — see the
[Disclaimer](../README.md#disclaimer).

**Who gets paid, and for what?**
Community health workers are paid a small amount of USDC on Stellar for each
patient they register and get verified. See
[Why This Matters](../README.md#why-this-matters).

**What if my information is wrong or a health worker verified something
incorrectly?**
A dispute process lets you (or anyone) flag a verified record; a committee
reviews and can remove the score/attestation locally and publish an override
on-chain. See the root README's
[Attestation & Trust Model](../README.md#attestation--trust-model).

**Is this a medical device?**
No — see the [Disclaimer](../README.md#disclaimer). HenBridge is an information
aid; treatment decisions remain the clinician's responsibility.

**Why isn't this just a regular database with logins?**
Because the point is that a responder with no login and no relationship to
the patient's home facility still needs a way to *trust* the data in
seconds. A plain database can't provide that; a tamper-evident,
independently checkable attestation can. See
[ADR 0001](adr/0001-why-stellar.md).
