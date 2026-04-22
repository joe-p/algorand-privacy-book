# ZK VMs

## TL;DR

- ZK VMs are a zero-knowledge proof of the *execution* of a standard ISA binary
  - Most commonly used ISA is RISC-V, but there are some custom ISAs
- ZK VMs allow ZK proofs to be generated from any language that can compile to the target ISA
  - For RISC-V, this means C, C++, Rust, etc.
- ZK VMs proofs are typically STARKs which are PQ
  - STARK proofs are large, so they are typically wrapped in a smaller proof such as groth16 or PLONK
  - The wrapped proof also enables ZK (the STARK is typically *not* ZK)
- ZK VMs can execute any program, but cannot have regular I/O (i.e. no networking)
- ZK VM proofs are computationally heavy to prove
  - Commonly need at least one GPU to prove in a reasonable time
  - There are also proving networks that can be used

## Privacy Properties

## Implementation Details

## Infrastructure Requirements

## Tooling
