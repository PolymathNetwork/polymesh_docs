## Overview

In Polymesh, all assets (excluding the network native token POLYX) are held at the identity level. This allows Polymesh to enforce compliance in real time based on claims also held at the identity level.

To allow users to organise their assets underneath their identity, and to flexibly assign key permissions and custody, Polymesh has the concept of portfolios.

## Portfolio Diagram

![Peer to peer](images/custody.png)

## Portfolios

Assets can be partitioned into logical portfolios within a single identity.

A particular asset can have different balances across portfolios within the same identity.

Compliance is applied to the sum of balances across an identities portfolios (it can also be applied across all identities under a [single entity](./confidential_identity.md)).

Permissions for keys can be applied at the portfolio granularity.

Portfolios are not related to compliance - i.e. claims remain at the identity level and are shared across all portfolios. Transfer restrictions are at the identity level, not per portfolio.

Transfers of assets between portfolios of the same identity should always be possible (provided the identity has a CDD check) and not subject to any compliance rules.

Signing keys are managed at the identity level, but can be granted access to specific portfolios under an identity.

The distribution of assets into portfolios, and the association of a portfolio with an identity are publicly stored on-chain.

Every identity has a default identity which is used in the case that a specific identity is not specified as part of a transfer or instruction settlement.

## Custody

A user can assign custodianship of a portfolio to another identity. The cleanly separates beneficial ownership of an asset (which always stays under the beneficiares identity) used for corporate actions, from custodial ownership where another entity may manage those assets on behalf of their beneficiary.

A portfolio can only be assigned to a single custodian at a time. Once assigned to a custodian, the custodian can:  

- affirm instructions that relate to the portfolio on behalf of the owner

- move assets out of the portfolio on behalf of the owner

- revoke their custodianship over the portfolio