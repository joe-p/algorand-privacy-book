# ZK SNARKs

## TL;DR

- SNARKs allow someone (the prover) to prove they know the inputs to a computable statement that makes it true to someone else (the verifier) without them having to run the full computation
  - This is the first useful part of SNARKs: *succinctness*. Proving is computationally heavy but verifying is not
  - This process is *noninteractive*, meaning the prover can prove the statement is true without the verifier needing to ask any questions
- A ZK SNARK allows the prover to prove the computable statement is true without revealing any additional information about the statement
  - This is the second useful part of SNARKs: *zero-knowledge*
- SNARKs work by turning a computable statement (commonly expressed in a domain-specific language) into a polynomial equation that can be solved by a prover and verified by a verifier
- The most common SNARKs require a trusted setup
  - MCP ceremony with a trust-one-of-many trust model so only one person needs to be honest
  - Some SNARKs don't require a ceremony, but these are more complex and don't yet have Algorand verifiers
- There's a wide variety of SNARK proof systems and tooling with various trade-offs

## Introduction

## Privacy Properties

## Implementation Details

## Infrastructure Requirements

## Tooling

### snarkjs-algorand

### AlgoPlonk
