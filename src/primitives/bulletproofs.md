# Bulletproofs

## TL;DR

- Bulletproofs are a ZK proof specifically for range checks
  - Commonly used with [Pedersen commitments](./pedersen.md)
- Similar proof size to PLONK (~600 bytes for uint64)
- Do not require any trusted setup
- Verification compute scales linearly with aggregation
  - This makes bulletproofs less desirable for use-cases with aggregation
- Expensive on AVM
  - Verification involves a very large multi-scalar multiplication that exceeds stack size
    - This requires multiple MSM calls, defeating the efficiency gains of MSM

## Introduction

## Privacy Properties

## Implementation Details

## Infrastructure Requirements

## Tooling
