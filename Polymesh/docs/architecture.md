## Overview

Polymesh is optimised for regulated assets and markets

Polymesh is a public, permissioned blockchain.

Implement core business logic and financial abstractions at the base layer.

Extendable primatives using constrained smart contracts called smart extensions.

Flexibility to allow layer 2 protocol development tightly integrated with primitives.

Privacy provided through confidential identity (V1) and confidential assets / positions (V2).

Polymesh is built on the Substrate framework.

POLYX

## Architecture Diagram

![Architecture Diagram](images/Polymesh Architecture.png)

## Pillars

### Identity

Identity is at the core of Polymesh. Polymesh implements a federated root of trust via permissioned Customer Due Diligence service providers. Every transaction in Polymesh is associated with an identity.

Identities in Polymesh are controlled via keys - each identity must have a single primary key and can optionally have additional secondary keys.

Identities are a collection of keys, claims and assets.

Provides pseudo-anonymous identity and key management. All users must act through an on-chain identity when interacting with Polymesh (with the exception of POLY transfers and the registration of new identities). Identities will be referenced through DIDs, following the w3c specification.

### Governance

### Confidentiality

### Compliance


## Asset

Allows users to issue, manage and transfer tokenised assets with regulatory compliance enforced on-chain. 

## Settlement

Facilitates trade settlement (e.g. atomic swap of different assets) and provides for issuers and investors to coordinate the sale or purchase of security tokens. This allows for both primary and secondary settlement.

## Capital Distribution

Provides a framework to distribute capital (e.g. USD tokens) to asset token holders. Distributed capital could be in the form of dividends, redemptions, liquidation events, interest payments or structured cashflows.

## Governance

Allows asset token holders to vote or otherwise participate on governance issues or corporate actions.

## Identity

Provides pseudo-anonymous identity and key management. All users must act through an on-chain identity when interacting with Polymesh (with the exception of POLY transfers and the registration of new identities). Identities will be referenced through DIDs, following the w3c specification.