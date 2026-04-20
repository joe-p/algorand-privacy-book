# Anonymous Mixer: Hermes Vault

## Privacy Architecture

The information on this page is based on [Hermes Vault commit 45c24e5](https://github.com/giuliop/HermesVault-smartcontracts/tree/45c24e5aaba325adb9fe393b4671d09316c07983)

### Overview

**Deposit**: Add `MiMC_Hash(Amount, K, R)` to merkle tree with the root stored in contract state. `K` and `R` remain secret.

**Withdraw**: Prove you know `Amount`, `K`, and `R` (without revealing them) via a circuit that calculates the same root in the contract via a merkle path (which is also private). You must also publicly reveal `MiMC_Hash(Amount, K)` as the nullifier to prove it has not yet been spent. All nullifiers are permanently stored in contract state. Partial amounts can be withdrawn by also creating a new deposit: `MiMC_Hash(Change, K2, R2)`.

> [!IMPORTANT]
> `MiMC_Hash(Amount, K, R)` and `MiMC_Hash(Amount, K)` are NOT linkable. If we only had one secret, the deposit commitment and nullifier would be one in the same.

### Methods

#### Deposit

##### ZK Public Inputs

| Signal | Description                |
| ------ | -------------------------- |
| Amount | The amount being deposited |

##### ZK Private Inputs

| Signal | Description      |
| ------ | ---------------- |
| K      | Nullifier secret |
| R      | Blinding secret  |

##### ZK Public Outputs

| Signal     | Description                            |
| ---------- | -------------------------------------- |
| Commitment | Commitment to the deposit with secrets |

##### ZK Circuit Logic

- Calculates `Commitment` via `MiMC_Hash(Amount, K, R)`

##### Contract State

| State       | Description                                                       |
| ----------- | ----------------------------------------------------------------- |
| Merkle Root | Root of the Merkle tree that contains all the deposit commitments |

##### Contract Logic

- Verifies `Amount` matches the amount sent to the contract atomically
- Adds `Commitment` to the tree and updates `Merkle Root`

#### Withdraw

##### ZK Public Inputs

| Signal     | Description                                                                                            |
| ---------- | ------------------------------------------------------------------------------------------------------ |
| Recipient  | The address the withdrawn amount must go to. Ensures withdrawals are not susceptible to front running. |
| Withdrawal | The amount being withdrawn                                                                             |
| Fee        | The amount being used to cover fees                                                                    |

##### ZK Private Inputs

| Signal | Description                                                               |
| ------ | ------------------------------------------------------------------------- |
| K      | Nullifier secret for the deposit being spent                              |
| R      | Blinding secret for the deposit being spent                               |
| Amount | The original amount that was deposited                                    |
| Change | The amount that is NOT being withdrawn and used to create a new "deposit" |
| K2     | Nullifier secret for the change deposit                                   |
| R2     | Blinding secret for the change deposit                                    |
| Index  | The index of the original deposit commitment in the Merkle tree           |
| Path   | The merkle path that proves the original deposit is in the Merkle tree    |

##### ZK Public Outputs

| Signal     | Description                                                            |
| ---------- | ---------------------------------------------------------------------- |
| Commitment | Commitment for the change deposit                                      |
| Nullifier  | Hash used to prove that the deposit hasn't already been spent          |
| Root       | The root of the merkle tree the original deposit commitment belongs to |

##### ZK Circuit Logic

- Calculates `Root` by using `MiMC_Hash(Amount, K, R)` as the leaf at `Index` with `Path`
- Calculates `Commitment` via `MiMC_Hash(Change, K2, R2)`
- Calculates `Nullifier` via `MiMC_Hash(Amount, K)`
- Verifies `Withdrawal + Change + Fee == Amount`

##### Contract State

| State       | Description                                                       |
| ----------- | ----------------------------------------------------------------- |
| Merkle Root | Root of the Merkle tree that contains all the deposit commitments |
| Nullifiers  | Set of all the nullifiers that have already been spent            |

##### Contract Logic

- Verifies `Nullifier` has not already been spent and adds it to `Nullifiers`
- Verifies `Root` matches the contract `Merkle Root`
- Adds new `Commitment` to the tree and updates `Merkle Root`
- Sends `Withdrawal` payment to `Recepient`
