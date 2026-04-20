# Fully Private Transactions: Mithras

## Privacy Architecture

### Overview

- Standard mixer (such as Hermes Vault) uses `Hash(amount, nullifier_secret, blinding_secret)` to commit to deposits
  - Anyone with `nullifier_secret` and `blinding_secret` can verify the `amount` AND spend it
- Mithras uses `Hash(amount, nullifier_secret, blinding_secret, receiver_public_key)`
  - Anyone with `nullifier_secret`, `blinding_secret`, and `receiver_public_key` can verify the `amount` but NOT spend it
  - Only the owner of `receiver_public_key` can spend via transaction signature verification
- We need senders to be able to derive new public keys that receivers own
  - We can't reuse the same address every time otherwise transactions can be linked
  - [HD xPub](../primitives/hd.md) not viable because it would allow all spenders sending to the same address see each other's transactions
    - Would also require off-chain sharing of xPub
  - [Stealth addresses](../primitives/stealth.md) used for *spend key* (Ed25519)
- To remove the need for off-chain sharing of secrets, [HPKE](../primitives/hpke.md) is used to encrypt `amount`, `nullifier_secret`, `blinding_secret`, and the scalar used to derive `receiver_public_key`
  - Encryption is performed with a *view key* (KEM algorithm in the HPKE suite, currently X25519)
    - Anyone that holds this key can decrypt secrets to view receiver and amount, but they CANNOT spend
  - If encryption is broken, secrets are revealed but funds are still safe due to signature verification
- In addition to `deposit` and `withdrawal`, Mithras has a `spend` method
  - This allow funds to move between users without revealing accounts OR amounts

> [!NOTE]
> Mithras addresses are composed of two keys: the *spend key* and the *view key*. Transactions in Mithras are fully private, but anyone with the *view key* can see the receiver and amounts.
