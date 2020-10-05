## Claim / Attestation

These terms are used interchangably. The Polymesh core code refers to claims, whilst the SDK and UIs refer to attestations. In both cases they represent an attestation from one identity about another identity. These attestations or claims are attached to an identity and allow asset issuers to manage compliance on their assets.

## Transaction

A transaction, also called extrinsic, is a user initiated on-chain action. All transactions carry a POLYX network fee and possible other costs. Transactions can fail if there are insufficient funds to pay for it, is not associated with a CDD'ed identity, or if it is deemed invalid by the networks logic.

## Customer Due Diligence (CDD)

Each Polymesh user have a CustomerDueDiligence claim associated with one of their identities to transact on the network. This claim is provided by a set of permissioned CDD service providers and is a light weight due diligence process designed to ensure all network participants can safely transaction with each other.

## Entity

An entity is a unique individual or organisation such as investors, asset issuers and service providers. An entity is represented on-chain by one or more identities

Polymath Unique Identity System (PUIS) generates a unique ID for each entity. i.e. an entity will always have exactly one unique ID in the PUIS.

## Identity

A collection of claims verifying an entity is who they say they are.

An entity can have multiple identities, which are tied together privately via the PUIS.

Each identity needs a name to reference it, which in our case is a Decentralised Identifier (DID) in the form did:poly:<pseudo_anonymous_id>.

## Portfolio

A digital wallet tied to an entity through an identity used to hold a collection of tokenised assets.

A single identity can hold multiple portfolios. A portfolio is owned by exactly one identity. Control of a portfolio can be passed to a custodian.

## Key

An authentication method for an identity, typically a public / private key pair, or multi-sig. 

Every identity has exactly one primary key, and can optionally have additional secondary keys.

A key can only be associated with a single identity concurrently.

Keys have permissioned access to their identity, at the portfolio, asset and action granularity.

POLYX balances are held at the key level - all other assets are held at the identity level.

## Venue

A venue is an organised trading facility (OTF), regulated markets (RMs), or multilateral trading facilities (MTFs) that receives a logical set of instructions from counter-parties for the purpose of matching, to convert separate orders into an executed trade

A venue is a logical groups of related instructions - one Venue can contain several Instructions

Venues define the details of where the pre-trade activities, if any, took place.

They also define which identities are allowed to sign receipts for off-chain transfers of assets.

## Instruction

An instruction is an atomic group of asset exchanges represented by legs

It records all of the counterparties associated with the instruction, and whether or not they have authorised the instruction.

It also records whether the instruction has been executed, rejected or failed.

An instruction is always settled atomically - i.e. all legs in the Instruction settle, or none of them do.

Each instruction specifies its expiry and valid_from details - authorisations and settlement can only happen during this time window.

## Leg

Represents a transfer of assets or stable coins from one counterparty to another

## Counterparty

The persons or institutions engaging in a settlement instruction. That is, the opposing entities involved in the execution of a securities contract.