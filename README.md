# [Cashtokens](https://cashtokens.cash/)
CashTokens: A proposal to enable two new primitives on Bitcoin Cash: fungible tokens and non-fungible tokens.

# CHIP-2022-02-CashTokens: Token Primitives for Bitcoin Cash
    Title: Token Primitives for Bitcoin Cash
    Type: Standards
    Layer: Consensus
    Maintainer: Jason Dreyzehner
    Status: Live
    Wallet: https://cashtokens.cash
    Initial Publication Date: 2022-02-22
    Latest Revision Date: 2023-05-03
    Latest Version: 2.2.1 (7552da2d)

## [CashTokens Web Wallet](https://cashtokens.cash/)

### Summary
This proposal enables two new primitives on Bitcoin Cash: fungible tokens and non-fungible tokens.

#### Terms
A token is an asset – distinct from the Bitcoin Cash currency – that can be created and transferred on the Bitcoin Cash network.

Non-Fungible tokens (NFTs) are a token type in which individual units cannot be merged or divided – each NFT contains a commitment, a short byte string attested to by the issuer of the NFT.

Fungible tokens are a token type in which individual units are undifferentiated – groups of fungible tokens can be freely divided and merged without tracking the identity of individual tokens (much like the Bitcoin Cash currency).

Deployment
Deployment of this specification is proposed for the May 2023 upgrade.

- Activation is proposed for 1668513600 MTP, (2022-11-15T12:00:00.000Z) on chipnet.
- Activation is proposed for 1684152000 MTP, (2023-05-15T12:00:00.000Z) on the BCH network (mainnet), testnet3, testnet4, and scalenet.

### Motivation
Bitcoin Cash contracts lack primitives for issuing messages that can be verified by other contracts, preventing the development of decentralized application ecosystems on Bitcoin Cash.

#### Contract-Issued Commitments
In the context of the Bitcoin Cash virtual machine (VM), a commitment can be defined as an irrevocable message that was provably issued by a particular entity. Two forms of commitments are currently available to Bitcoin Cash contracts:

- Transaction signatures – a commitment made by a private key attesting to the signing serialization of a transaction.
- Data signatures – a commitment made by a private key attesting to the hash of an arbitrary message (introduced in 2018 by OP_CHECKDATASIG).
Each of these commitment types require the presence of a trusted private key. Because contracts cannot themselves hold a private key, any use case that requires a contract to issue a verifiable message must necessarily rely on trusted entities to provide signatures1. This limitation prevents Bitcoin Cash contracts from offering or using decentralized oracles – multiparty consensus systems that produce verifiable messages upon which other contracts can act.

By providing a commitment primitive that can be used directly by contracts, the Bitcoin Cash contract system can support advanced, decentralized applications without increasing transaction or block validation costs.

#### Numeric Commitments
The Bitcoin Cash virtual machine (VM) supports two primary data types in VM bytecode evaluation: byte strings and numbers. Given the existence of a primitive allowing contracts to commit to byte strings, another commitment primitive can be inferred: numeric commitments.

Numeric commitments are a specialization of byte-string commitments – they are commitments with numeric values which can be divided and merged in the same way as the Bitcoin Cash currency. With numeric commitments, contracts can efficiently represent fractional parts of abstract concepts – shares, pegged assets, bonds, loans, options, tickets, loyalty points, voting outcomes, etc.

While many use cases for numeric commitments can be emulated with only byte-string commitments, a numeric primitive enables many contracts to reduce or offload state management altogether (e.g. shareholder voting), simplifying contract audits and reducing transaction sizes.

In this proposal, numeric commitments are called fungible tokens.

### Benefits
By enabling token primitives on Bitcoin Cash, this proposal offers several benefits.

#### Cross-Contract Interfaces
Using non-fungible tokens (NFTs), contracts can create messages that can be read by other contracts. These messages are impersonation-proof: other contracts can safely read and act on the commitment, certain that it was produced by the claimed contract.

With contract interoperability, behavior can be broken into clusters of smaller, coordinating contracts, reducing transaction sizes. This interoperability further enables covenants to communicate over public interfaces, allowing diverse ecosystems of compatible covenants to work together, even when developed and deployed separately.

Critically, this cross-contract interaction can be achieved within the "stateless" transaction model employed by Bitcoin Cash, rather than coordinating via shared global state. This allows Bitcoin Cash to support comparable contract functionality while retaining its >1000x efficiency advantage in transaction and block validation.

#### Universal Token Primitives
By exposing basic, consensus-validated token primitives, this proposal supports the development of higher-level, interoperable token standards (e.g. SLP). Token primitives can be held by any contract, wallets can easily verify the authenticity of a token or group of tokens, and tokens cannot be inadvertently destroyed by wallet software that does not support tokens.
