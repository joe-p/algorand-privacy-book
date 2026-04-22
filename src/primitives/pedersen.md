# Pederson Commitments

## TL;DR

- Pederson commitments enable homomorphic addition
  - Encrypted values can be added without revealing any of the values
- Commonly used for confidential balances and transactions
- Pederson commitments require range proofs
  - Need a ZK proof to prove that the value is not negative
  - On Algorand, this would typically be a [ZK SNARK](./zksnarks.md)
  - [Bulletproofs](./bulletproofs.md) are commonly used for range proofs but are more expensive on AVM

## Introduction

## Privacy Properties

## Implementation Details

## Infrastructure Requirements

## Tooling
