# Introduction

This book covers privacy primitives available to developers building on Algorand along with some example applications. This introductory page outlines some important things to think about with regards to privacy.

## How We Talk About Privacy

### Dimensions of Privacy

For most blockchain use cases, there are two dimensions of privacy.

#### Confidentiality: Transaction Amount

Confidentiality means transactions amounts are not publicly revealed. A system with confidential transactions amounts typically implies that the balances of the users in the system are also confidential.

#### Anonymity: Transacting Parties

Anonymity means that the parties involved in the transaction (i.e. sender and receiver) are not publicly revealed.

#### Privacy: Metadata

The final dimension of privacy is the metadata of transactions. This could involve pieces of information such as memos, linked transactions, locations, etc.

### Degrees of Privacy

## Other Important Considerations

#### Selective Disclosure

In real-world products, selective disclosure is often needed to allow multiple parties to view otherwise private information. Some common examples are auditors or regulators that need to be able to view transactions that are private to any external observer. Whether or not selective disclosure is possible largely depends on the underlying privacy primitives used. Different primitives enable selective disclosure in different ways and they each have their own trust models.

#### Implementation Complexity

An important consideration for using privacy primitives or applications is understanding of the implementation complexity. There are some privacy primitives that are very powerful, but also very complex. This means the architecture of an app can be sound, but a small implementation detail may create privacy or security problems.

#### Infrastructure Complexity

Another important consideration for adoption of privacy applications is the infrastructure requirement for using said applications. For example, some privacy solutions require off-chain state synchronization which can significantly increase the infrastructure burden compared to a solution without any privacy

#### Cryptographic Security

Most privacy primitives rely on advanced cryptographic techniques. Understanding the security model of these primitives is crucial for understanding the security model of an application that uses the primitives. For example, many primitives rely on Elliptic-Curve cryptography, which can be broken by sufficient quantum computers. Care should be taken to not only have a clear understanding of the security model today, but also have a clear understanding of the security model in the future.
