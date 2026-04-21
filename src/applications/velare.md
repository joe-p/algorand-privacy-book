# Confidential Amounts: Velare

## Privacy Architecture

### Overview

Velare is a protocol that enables confidential transaction amounts (and thus confidential balances).

Velare was created to reduce the infrastructure requirements. Namely:

- Both [Mithras](./mithras.md) and [Hermes Vault](./hermes.md) require a Merkle tree which requires *either*:
  - Storing the entire tree in boxes adding an extra ~0.012 ALGO per txn and potentially GBs of data permanently in the ledger
  - Clients to follow the chain the reconstruct the Merkle tree off-chain
- [Mithras](./mithras.md) uses [stealth addresses](../primitives/stealth.md) which requires
  - Clients to follow the chain and check every transaction to see if its intended for them

By eliminating the Merkle tree and stealth addresses we can dramatically simplify the require infrastructure and contract methods.

#### Deposit

- Create a UTXO commitment in a ZK circuit `MiMC_Hash(amount, asset, secret, receiver)` without revealing `secret`.
  - `receiver` is a hash of an ed25519 spending public key and an [HPKE](../primitives/hpke.md) KEM public key (for now, X25519).
- The secret is encrypted via [HPKE](../primitives/hpke.md)
  - The ciphertext stored in a box that uses the `receiver` and `asset` as the key so it can be easily retrieved from algod.

#### Spend

- In a ZK circuit, prove you know the values of `MiMC_Hash(amount, asset, secret, receiver)`.
  - `receiver` is revealed to the contract which verifies that the transaction sender is `receiver`
  - `amount`, `asset`, and `secret` remain secret
  - The contract verifies this commitment exists in a box and then deletes it ensuring it is only spent once
- In the same ZK circuit, create a new commitment `MiMC_Hash(out_amount, asset, out_secret, out_receiver)`
  - This commitment is added to a box for `out_receiver`
  - `out_amount`, `asset`, and `out_secret` remain secret

#### Withdraw

- Same as spend, except there is one output where `amount` and `asset` are also revealed alongside `receiver`
  - The contract issues an inner transaction to send the asset to the specified address

> [!NOTE]
> For the sake of simplicity in the overview there is just one input and one output. In reality, there are multiple circuits for various combinations of inputs and outputs. The most common is 2 in and 2 out.
