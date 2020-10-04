## Overview

In many places in Polymesh two identities, or a key and an identity, need to invite each other to have certain types of permission or access.

For example, an identity may want to invite a key to join its identity as a secondary key.

Authorisations in Polymesh allow an identity to invite another identity or key to take some privileged action.

An identity or key can view any authorisations that they may have pending, and can approve or reject each of these authorisations.

## Authorisation Types

There are various types of authorisations that Polymesh allows:  

```
pub enum AuthorizationData<AccountId> {
    /// CDD provider's attestation to change primary key
    AttestPrimaryKeyRotation(IdentityId),
    /// Authorization to change primary key
    RotatePrimaryKey(IdentityId),
    /// Authorization to transfer a ticker
    /// Must be issued by the current owner of the ticker
    TransferTicker(Ticker),
    /// Authorization to transfer a token's primary issuance agent.
    /// Must be issued by the current owner of the token
    TransferPrimaryIssuanceAgent(Ticker),
    /// Add a signer to multisig
    /// Must be issued to the identity that created the ms (and therefore owns it permanently)
    AddMultiSigSigner(AccountId),
    /// Authorization to transfer a token's ownership
    /// Must be issued by the current owner of the asset
    TransferAssetOwnership(Ticker),
    /// Authorization to join an Identity
    /// Must be issued by the identity which is being joined
    JoinIdentity(Vec<Permission>),
    /// Authorization to take custody of a portfolio
    PortfolioCustody(PortfolioId),
    /// Any other authorization
    /// TODO: Is this used?
    Custom(Ticker),
    /// No authorization data
    NoData,
}
```

Each of these authorisations are issued by an identity, but take different parameters depending on the type of action taking place.

## Payment for Authorisation

In some cases it may not be possible for the approver (or rejected) of an authorisation to pay for the transaction to do so. When a key is joining a new identity, it may not have any POLYX funds to pay for the transaction, and cannot receive POLYX as it is not yet associated with an identity (with a valid CDD claim).

When a multisig signer is proposing or approving a multisig transaction it may similarily not have POLYX (a multisig signer cannot receive POLYX as it is not directly connected to an identity).

So for the certain types of authorisation below, the transaction is paid for by the issuer of the authorisation, in particular the primary key of the identity that issued the authorisation.

Authorisations types where the authorisation issuer pays are: 

- JoinIdentity

- RotatePrimaryKey

- AddMultiSigSigner