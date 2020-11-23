## Overview

Assets on Polymesh can represent any type of digitalised asset, and are originated and managed through the asset base layer logic in Polymesh.

This ensures that all assets are created in a standardised manner, allowing related functionality such as corporate actions, settlement and compliance, to function seamlessly across all assets.

Once an asset has been created, its ownership is represented via balances of that assets tokens held by different investors.

Ownership of an asset on Polymesh can be determined via its:

- total supply: the total number of tokens that represent ownership in the asset

- investor balances: the individual balance of each investing identity in the asset

The asset pallet documentation can be found at:
<https://docs.polymesh.live/pallet_asset/index.html>

Polymesh allows you to manage the full lifecycle of any asset directly on the Polymesh blockchain, including the issuance, initial distribution or fundraise of the asset and any subsequent corporate actions such as dividend payments, capital distributions or corporate ballots.

Asset tokens can be either divisible or indivisible.

## Ticker Registration

Every asset on Polymesh has a unique ticker - a 12 character identifier for the asset.

Users can register a ticker, and then when they are ready create an asset associated with that registered ticker.

Ticker registrations can be transferred between identities and have an expiry date, currently set to 6 months from their initial registration.

Registering a ticker ensures no other user can issue an asset with that ticker whilst the registration is valid (i.e. has not expired).

## Asset Creation

Having registered a ticker, users can then create their asset, specifying the type of the asset (e.g. Equity, Bond, Fund).

The asset is initially created with a zero total supply. For the initial issuance of the asset, see the Issuance section.

As well as being identified via a unique ticker, assets can be associated with additional external identifiers, such as ISINs, CUSIPs, CINs, LEIs and DTIs, with Polymesh validating that these identifiers are consistent (i.e. that their checksums match).

If a ticker is not currently reserved, the ticker registration is done automatically when a user creates an asset.

## Documentation

Polymesh allows the issuer of an asset to associate documents with that asset.

The actual document is not stored on-chain, and instead the asset is associated with a document reference on-chain, where the reference includes a name, the URL at which the document can be found (this may be a permissioned URL requiring an investor in the asset to have some credentials to access the document), as well as an optional document type, filing date, and hash of the document contents.

Documentation can only be updated or modified by the identity that issued the corresponding asset (the asset issuer).

## Ownership

All asset ownership in Polymesh is at the identity granularity. Whilst an investor can organise their assets into different portfolios under their [identity](./identity.md), compliance is enforced at the identity level, so in order to pay or receive a particular assets token, the relevant identity must have claims that match the [compliance](./compliance.cd) rules specified by the asset issuer for the asset.

## Asset Issuers

The asset issuer is the identity which registered the ticker and created the asset. The asset issuer can transfer ownership of an asset to another identity by issuing a `TransferAsset` authorisation which must be accepted by the target identity.

The asset issuer also has some additional controls which are soley accessible to them. These include:

- updating documentation and identifiers for their asset

- freeze and unfreeze any transfers of their asset

- add and remove compliance rules for their asset

- permission venues to settle their asset

In addition an asset issuer can execute a controller transfer of their token. This allows them to force transfer ownership of their asset tokens from any investor back to the primary issuance agent of the asset. The primary issuance agent is an identity which an asset issuer specifies for their asset, and is responsible for treasury management and token distribution.

## Issuance

The issuance process for assets in Polymesh allows the originator of an asset (the asset issuer) to issue and distribute their asset to investors.

### Roles

Every asset, identified by its unique ticker, is associated with two identities:

- the asset issuers identity: this is the identity that created the asset

- the primary issuance agents identity: this is the identity responsible for distributing the asset to investors.

These identities can be the same, and the primary issuance agent identity is in fact defaulted to the asset issuers identity.

The asset issuer can set the primary issuance agent for their asset to any other identity, although the targetted identity must accept this role via a `AcceptPIA` authorisation. In the case where the primary issuance agent is the asset issuer, no authorisation is needed.

### Process

Once an asset issuer has created and configured their asset, they can then issue tokens representing ownership in the asset to the primary issuance agent.

The primary issuance agent can then distribute those asset tokens to investors directly or via an security token offering, in both cases using the settlement engine.

This approach allows a clean separation between the issuance process, which bypasses both the compliance and settlement engine and is restricted to only issuance to the configured primary issuance agent, and the distribution process which uses both the compliance and settlement engines.

## Checkpoints

An _asset checkpoint_ is a collection of records representing the balances of that asset at a given
time. The balances recorded are the total asset balance and all the balances held by identities.

This is useful for capital distributions and corporate ballots where we need a consistent set of balances as of some specific time (or block).

### Creating a checkpoint

There are two ways to create a checkpoint.

1. Call the dispatchable `create_checkpoint(ticker)` in the `asset` pallet. This creates a
   checkpoint for the asset of `ticker`. The total balance is recorded instantly, and each balance
   held an identity is recorded just before the next transaction to or from that identity. Since the
   balances of identities are not recorded instantly, we call this process _lazy checkpointing_. A
   consequence of this method is that no records are made if there are no transactions.

2. Create a checkpoint schedule using the dispatchable `create_checkpoint_schedule(ticker,
   schedule)` in the same pallet. A schedule consists of a starting timestamp in [seconds since the
   UNIX epoch][unix_time] and a period for recurring the checkpoint at regular intervals. The period
   is a multiple of one of seconds, minutes, hours, days, weeks, months, or years. If the period has
   zero length then the checkpoint schedule it defines is non-recurring and the only checkpoint it
   contains is the one at the start. Scheduled checkpoints are created automatically at times
   defined by the schedule but are otherwise identical in nature to manual checkpoints. Scheduling
   of the next checkpoint happens lazily as well, on an asset transaction or minting. If there are
   no such transactions by the time a checkpoint is due, it does not get rescheduled, which is fine
   with the scheduler since it takes into account any missed checkpoints.


### Checkpoint periods

There are two kinds of periods: ones of fixed length if measured in seconds -- multiples of seconds,
minutes, hours, days or weeks -- and ones of variable length -- multiples of months or years.

When computing the time when the next checkpoint is due, if the period is fixed the runtime adds the
length of that period in seconds to the time when the last checkpoint was due.

If the period is variable, the next timestamp computation takes into account the day of the month at
the start of the schedule. Every checkpoint gets a day of the month that is at most that of the
start of the schedule.

For example, assume we have the checkpoint schedule that

* starts on 00:01:00 the 31st of January, 2021 UTC and

* has a period of one month.

The checkpoint timestamps are going to be

* 2021-01-31 00:01:00

* 2021-02-28 00:01:00

* 2021-03-31 00:01:00

* 2020-04-30 00:01:00

and so on.

### Accessing an existing checkpoint

The checkpoints of an asset form a sequence that is indexed starting from 1. The total balance and
balances of identities are stored at these indices.

To read the total supply of `ticker` at checkpoint index `i` one calls the getter
`total_supply_at(ticker, i)` in the asset pallet. The getter returns the value directly from runtime
storage.

To read the checkpoint balance of `identity` one calls the function `get_balance_at(ticker,
identity, i)` in the same pallet. This function returns the checkpoint balance at `i` by searching
for the least checkpoint index greater or equal to `i` or, if no such record exists due to the
absence of transactions, the current asset balance of `identity`.

The time at which a checkpoint was made is stored in a map indexed by checkpoint sequence numbers
and can be read knowing the required sequence number.

[unix_time]: https://en.wikipedia.org/wiki/Unix_time
