# HD Accounts

## Introduction

Hierarchical Deterministic Accounts (HD Accounts) allow multiple accounts to be derived from a single secret key. This is done via a chain of cryptographic operations performed on the root secret key. Each derivation step can be one of two types of derivations: soft or hard.

### Hard Derivation

Hard derivation is a derivation step that derives a new ed25519 keypair that cannot be linked to the parent or sibling keypairs. In terms of capabilities, an HD account derived via hard derivation has the same capabilities as a regular ed25519 keypair (with the exception of being able to derive child accounts).

### Soft Derivation

Soft derivation is a derivation step that derives a special key pair, commonly called an extended key pair. The extended public key (xPub) can be used by anyone that has it to derive child accounts. The private key for these accounts can only be derived by the owner of the extended private key (xPriv).

## Privacy Properties

HD derivation enables a user to derive new accounts with any cadence (for example, one new account per transaction). This alone can technically be done with regular ed25519 keypairs, but HD accounts means only one secret key needs to be stored. Additionally, the xPub enables the ability to have selective disclosure of account linking. For example, Alice can generate a xPub, *xPub<sub>Alice</sub>*, and shared it with Bob. Bob can then derive an new public key using *xPub<sub>Alice</sub>*. Both Alice and Bob will know that this is a new account that Alice controls. To an external observer it will appear as if Bob sent a transaction to a random account without being able to link it back to Alice.

> [!WARNING]
> It's important to note that the xPub is partially private information. The xPub being leaked will result in all past and future child accounts being linked to each other and the parent account.

## Implementation Details

TODO

## Infrastructure Requirements

Key management of HD accounts is not very different than regular ed25519 keypairs. The main difference is that the extended private keys (including the root key) is 96 bytes instead of 64 bytes.

## Tooling

### xHD-Wallet-API-ts

[xHD-Wallet-API-ts](https://github.com/algorandfoundation/xHD-Wallet-API-ts) is a TypeScript library that enables HD account derivation and signing. It is compatible with web, Node, and React Native.

### xHD-Wallet-API-rs

[xHD-Wallet-API-rs](https://github.com/algorandfoundation/xHD-Wallet-API-rs) is a Rust library that enables HD account derivation and signing.

### xHD-Wallet-API-py

[xHD-Wallet-API-py](https://github.com/algorandfoundation/xHD-Wallet-API-py) is a Python library with bindings to the xHD-Wallet-API-rs for HD account derivation and signing.

### xHD-Wallet-API-swift

[xHD-Wallet-API-swift](https://github.com/algorandfoundation/xHD-Wallet-API-swift) is a Swift library that enables HD account derivation and signing.

### xHD-Wallet-API-kt

[xHD-Wallet-API-kt](https://github.com/algorandfoundation/xHD-Wallet-API-kt) is a Kotlin library that enables HD account derivation and signing.
