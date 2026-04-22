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

- Prove you know `amount`, `asset`, `secret`, and `receiver` and reveal commitment `MiMC_Hash(amount, asset, secret, receiver)`
  - `secret` remains secret
  - `amount`, `asset`, and `receiver` are all revealed to the contract

#### Spend

> [!NOTE]
> Velare supports multiple inputs and outputs when spending. `IN` is the amount of inputs and `OUT` is the amount of outputs.

- Prove you know `in_amounts[IN]`, `asset`, `in_secrets[IN]`, and `receiver` and reveal `IN` commitments `MiMC_Hash(in_amounts[IN], asset, secrets[IN], receiver)`
  - `in_amounts[IN]`, `asset`, and `in_secrets[IN]` remain secret
  - `receiver` is revealed to the contract which verifies that the transaction sender is `receiver`
  - The contract verifies each commitment exists in a box and then deletes it ensuring it is only spent once
- Prove you know `out_amounts[OUT]`, `asset`, `out_secrets[OUT]`, and `out_receivers[OUT]` and reveal `OUT` commitments `MiMC_Hash(out_amounts[OUT], asset, out_secrets[OUT], out_receivers[OUT])`
  - `out_amounts[OUT]`, `asset`, and `out_secrets[OUT]` remain secret
  - `out_receivers[OUT]` is revealed to the contract so `OUT` commitments can be stored in boxes
- Prove `Sum(out_amounts[OUT]) == Sum(in_amounts[IN])`
  - Recall `out_amounts[OUT]` and `in_amounts[IN]` remain secret

#### Withdraw

- Same as spend, except there is one output where `amount` and `asset` are also revealed alongside `receiver`
  - The contract issues an inner transaction to send the asset to the specified address
