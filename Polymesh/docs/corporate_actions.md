## Overview

[CA]: https://en.wikipedia.org/wiki/Corporate_action
[AGM]: https://en.wikipedia.org/wiki/Annual_general_meeting
[asset]: https://github.com/PolymathNetwork/polymesh_docs/blob/master/Polymesh/docs/assets.md
[pallet_ca]:  https://docs.polymesh.live/pallet_corporate_actions/index.html
[pallet_ballot]: https://docs.polymesh.live/pallet_corporate_actions/ballot/index.html
[pallet_dist]: https://docs.polymesh.live/pallet_corporate_actions/distribution/index.html
[checkpoints]: https://github.com/PolymathNetwork/polymesh_docs/blob/master/Polymesh/docs/assets.md#checkpoints
[`initiate_corporate_action`]: https://docs.polymesh.live/pallet_corporate_actions/enum.Call.html#variant.initiate_corporate_action
[`reset_caa`]: https://docs.polymesh.live/pallet_corporate_actions/enum.Call.html#variant.remove_ca
[`change_record_date`]: https://docs.polymesh.live/pallet_corporate_actions/enum.Call.html#variant.change_record_date
[`set_default_withholding_tax`]: https://docs.polymesh.live/pallet_corporate_actions/enum.Call.html#variant.set_default_withholding_tax
[`set_did_withholding_tax`]: https://docs.polymesh.live/pallet_corporate_actions/enum.Call.html#variant.set_did_withholding_tax
[`set_default_targets`]: https://docs.polymesh.live/pallet_corporate_actions/enum.Call.html#variant.set_default_targets
[`link_ca_doc`]: https://docs.polymesh.live/pallet_corporate_actions/enum.Call.html#variant.link_ca_doc
[`distribute`]: https://docs.polymesh.live/pallet_corporate_actions/distribution/enum.Call.html#variant.distribute
[`claim`]: https://docs.polymesh.live/pallet_corporate_actions/distribution/enum.Call.html#variant.claim
[`push_benefit`]: https://docs.polymesh.live/pallet_corporate_actions/distribution/enum.Call.html#variant.push_benefit
[`reclaim`]:
https://docs.polymesh.live/pallet_corporate_actions/distribution/enum.Call.html#variant.reclaim
[`remove_distribution`]:
https://docs.polymesh.live/pallet_corporate_actions/distribution/enum.Call.html#variant.remove_distribution
[`attach_ballot`]: https://docs.polymesh.live/pallet_corporate_actions/ballot/enum.Call.html#variant.attach_ballot
[RCV]: https://en.wikipedia.org/wiki/Ranked_voting
[`change_end`]: https://docs.polymesh.live/pallet_corporate_actions/ballot/enum.Call.html#variant.change_end
[`change_meta`]: https://docs.polymesh.live/pallet_corporate_actions/ballot/enum.Call.html#variant.change_meta
[`change_rcv`]: https://docs.polymesh.live/pallet_corporate_actions/ballot/enum.Call.html#variant.change_rcv
[`remove_ballot`]: https://docs.polymesh.live/pallet_corporate_actions/ballot/enum.Call.html#variant.remove_ballot
[`vote`]: https://docs.polymesh.live/pallet_corporate_actions/ballot/enum.Call.html#variant.vote

In the lifecycle of publicly traded companies, e.g., stocks in ACME Inc.,
there are what is known as [*corporate actions* (CAs)][CA].
Examples of these include dividend payments, stock splits,
and ballot for the [*annual general meeting* (AGM)][AGM].

Assuming ACME has been digitized as an [asset through that pallet][asset],
Polymesh can manage some types of CAs as well for you.
This is handled through three pallets:

1. [Corporate Actions][pallet_ca], which is the base layer handling setups, CA initiation, documents linking, and removal.
2. [Ballots][pallet_ballot], through which corporate ballots for AGMs can be conducted.
3. [Capital Distributions][pallet_dist], through which benefits,
   e.g., dividends, can be distributed to investors.

Now let's dig into the details!

### Initiating a corporate action

All CAs start with [`initiate_corporate_action`],
a dispatchable function that creates a CA.
When creating the CA, the associated asset's ticker, free-form text,
and the CA kind, e.g., a notice or a benefit need to be provided.

Additionally, a record date, withholding taxes,
and the targets of the CA can be specified.
For more on those, read on.

#### Corporate Action Agent (CAA)

The function must be called by the asset's *corporate action agent* (CAA).
By default, the CAA is the asset's owner.
Using the authorization system, this can be changed, assigning someone else as CAA.

When the owner is not CAA, they can no longer initiate CAs,
tweak the setup, ballots, distributions, or related functionality.
Who the CAA is can be reset, however, using the dispatchable [`reset_caa`].

#### Record dates & checkpoints

Supposing that ACME is traded frequently,
it would be difficult to know the entitlement of each investor in a distribution,
or how much voting power each holder can use in an ballot.
To provide these services with stable reference points,
the CA pallet uses [checkpoints] to determine an investor's balance and the total supply.
For CAs, the checkpoint used is known as the *record date*,
and is specified when creating the CA.

A record date can be specified in one of three ways:
1. By providing an existing checkpoint, created in the past.
2. Specifying a scheduled time at which a checkpoint should be created.
3. Providing an existing irremovable schedule.

Once a CA has been initiated,
assuming the services attached to the CA, such as a ballot or a distribution, permit,
the record date can still be changed using the dispatchable [`change_record_date`].

#### Taxes

Some CAs have taxable components.
Chief among them are dividends, where the investor receives income by holding the asset.
In some jurisdictions, it is the responsibility of the issuer,
or the CAA to deduct tax from any dividends and send it to the relevant government.
For example, if ACME is based in Sweden, 20% tax would apply.

Polymesh can handle this *withholding tax* natively,
allowing you to specify how many % that should be withheld.
This % can either be specified generally for all investors
(the *default withholding tax*),
or customized to specific individual investors (*DID-specific withholding tax*).

When the same taxes are used over and over again for many CAs,
the process can be simplified by specifying asset level defaults,
This is achieved using [`set_default_withholding_tax`] and [`set_did_withholding_tax`],
where the former is general and the latter is DID-specific.

#### Targeted investors

By default, a CA for some asset will apply to,
or *target*, every investor holding any asset balance.
Sometimes however, for whatever reason, some identities must be excluded from the target set.
When creating a CA, the target set can either be specified inclusively,
or alternatively, a set of identities can be excluded explicitly.
As with taxes, an asset level default can also be set, using [`set_default_targets`].

#### Linked documentation

Where the free-form text is insufficient,
or existing documentation relevant for the CA already exists,
asset documentation can instead be linked to the CA.
Supposing that both a CA and asset documents already exist,
they can be related using [`link_ca_doc`], linking the CA to a list of documents.

### Capital distribution

Polymesh can do more than merely record CAs on-chain.
A capital distribution, such as paying a dividend to investors, can also be handled natively.
Once CA of the predictable or unpredictable benefit kind has been created,
the dispatchable [`distribute`], in the [capital distributions pallet][pallet_dist],
is used to attach a distribution to the CA.

When calling `distribute`, the CA,
an amount of a currency to distribute from a portfolio of the CAA,
and the start / expiry dates of the distribution, are specified.

Once a distribution exists, and the start time is due,
investors can use [`claim`] before any expiry date to pull their benefit to their default portfolio.
However, the CAA, or the owner, can also use [`push_benefit`] to push benefits to investors.
Once the distribution has expired, the distribution creator can [`reclaim`] any remaining balance.

Before a distribution has started, it can also be cancelled, using [`remove_distribution`].

### Corporate ballots

Polymesh can also handle ballots on-chain.
For example, a corporation's annual general meeting can be conducted using ballots.

#### Ballot structure

A ballot has a start and end date,
during which investors can vote on motions included in the ballot.
These motions in turn are associated with descriptions and links to more information.

More importantly, however, motions contain choices,
e.g., a) *"aye"*, b) *"aye with X amendment"*, and c) *"nay"*, to which weights can be assigned.
These weights or votes are based on how many tokens each investor holds.
For example, an investor with 200 tokens will have 200 votes and one with 100 tokens has 100.
Those 200 votes could be assigned like so: a) 50, b) 150, c) 0.

Motions are independent of each other.
For example, consider a vote on who should be in the board of directors,
and a motion to discharge the old board from liability.
These would be independent, and therefore, all 200 tokens can be reused on each motion.

Optionally, a ballot can be configured with [*ranked-choice voting (RCV)*][RCV].
This allows voters to specify that the weight,
assigned to a choice not making it,
should fall back to a different choice.
For example, consider 3 choices paired with weights:
- a) 49 votes, no fallback
- b) 100 votes, fallback to a)
- c) 120 votes, no fallback.

In this case, c) initially has most weight assigned to it.
However, once a) has been discarded, the 49 votes it would move over to b),
which now has 149 votes in total. Therefore, b) becomes the adopted choice.

#### How-to

Now let's go over how a ballot can be made, tweaked, and voted on.
Once a CA of the notice kind has been created,
the a ballot can be attached using [`attach_ballot`].
This function will take the aforementioned details such as the motion data.

After the ballot has been created, but before voting has started,
the ballot's configuration may be amended, in 3 ways:
- [`change_end`] to change when no more votes will be accepted,
- [`change_meta`] to change the title and the motions,
- and [`change_rcv`] to change whether RCV is supported or not.

Moreover, before the start, the ballot can be removed using [`remove_ballot`].

Once the start date is due, any token holder can use [`vote`] to assign votes to all choices.
Investors can vote as many times as they want, regretting their choices, until the end is due.
