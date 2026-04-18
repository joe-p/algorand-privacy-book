# Hybrid Public Key Encryption (HPKE)

## TL;DR

- HPKE enables encryption between two parties via asymmetric key pairs
- If the blockchain is used to transmit a message, it should be assumed an attacker could *eventually* decrypt the message
  - This is especially true if using elliptic curve cryptography (i.e. X25519 for KEM)
  - Ratcheting, as seen in [double ratchet](https://signal.org/docs/specifications/doubleratchet/) or [MLS](https://www.rfc-editor.org/rfc/rfc9420.html), can be used to mitigate this risk

## Introduction

## Privacy Properties

## Implementation Details

## Infrastructure Requirements

## Tooling
