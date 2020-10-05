## Overview

Polymesh allows asset issuers to enforce relevant compliance on their assets in real-time, through the use of claim based transfer rules, and optional additional transfer restriction smart extensions.

The compliance manager, implemented in the base layer of the Polymesh blockchain, provides a flexible framework to allow asset issuers to easily configure complex transfer rules based on claims that must be held by their investors.

Smart extensions allow asset issuers to add additional transfer restrictions that depend on other attributes, such as the total number of investors or the amount of assets held by a particular investor.

## Claims

There are several types of claims that can be attested from one identity to another. A claim should be interpreted simply as something which is being claimed, and does not have any proof associated with it in general.

The full list of possible claim types are:  

```
pub enum ClaimType {
    /// User is Accredited
    Accredited,
    /// User is Accredited
    Affiliate,
    /// User has an active BuyLockup (end date defined in claim expiry)
    BuyLockup,
    /// User has an active SellLockup (date defined in claim expiry)
    SellLockup,
    /// User has passed CDD
    CustomerDueDiligence,
    /// User is KYC'd
    KnowYourCustomer,
    /// This claim contains a string that represents the jurisdiction of the user
    Jurisdiction,
    /// User is exempted
    Exempted,
    /// User is Blocked.
    Blocked,
    ///
    InvestorZKProof,
    /// Empty type
    NoType,
}
```

Of these, there are two "special" claims which are treated differently, which are:  

- `CustomerDueDiligence`: This can only be issued by trusted [CDD](./cdd.md) service providers and allows a user general access to the Polymesh network.

- `InvestorZKProof`: These are self-issued claims (an identity makes the claim about itself) that declare any [linkage](./confidential_identity.md) between identities owned by the same entity investing in the same asset.

The remainder of the claims can be issued by anyone against any identity. Some of these claim types are parameterised with additional information relevant to the claim, for example `Jurisdiction` includes the country code of the users jurisdiction.

Each claim has an expiry, after which the claim is no longer considered valid. For `BuyLockup` and `SellLockup` this can be used to prevent investors from buying or selling an asset until after the expiry date of the respective claim.

## Trusted Claim Issuers and Scopes

The compliance manager allows asset issuers to configure the compliance rules for their asset.

Asset issuers start by specifying which other identities they trust for the purposes of assessing claims on their investors. This allows them to set rules which are only satisfied if an investor has the relevant claims, issued by these trusted identities. An asset issuer can include themselves in this list of trusted claim issuers, as well as their partnered KYC organisations.

Asset issuers also specify a scope for their asset. This scope allows their trusted claim issuers to issue claims that directly target the asset, by linking the claim to the assets scope. In the case of `InvestorZKProof` this scope is the granularity at which investors are required to link their identities (i.e. any identity that wishes to invest in an asset with a scope of X, must self-declare an `InvestorZKProof` linking any identities investing in an asset with scope X).

## Compliance Rules

Asset issuers can specify compliance requirements for their assets. If both parties of a settlement leg satisfy these requirements, then the leg can be settled as part of its instruction.

Each individual compliance requirement consists of a list of conditions that the sender must satisfy, and a list of conditions that the receiver must satisfy:  

```
pub struct ComplianceRequirement {
    pub sender_conditions: Vec<Condition>,
    pub receiver_conditions: Vec<Condition>,
    /// Unique identifier of the compliance requirement
    pub id: u32,
}
```

All of the conditions for a particular compliance requirement (for both the sender and receiver) must be satisfied, although it is possible for these lists to be empty.

An asset issuer can choose to also pause compliance on their asset:  

```
pub struct AssetCompliance {
    /// This flag indicates if asset compliance should be enforced
    pub paused: bool,
    /// List of compliance requirements.
    pub requirements: Vec<ComplianceRequirement>,
}
```

If any of the compliance requirements are satisfied in a particular leg settlement, then the compliance is considered to be complete. This allows asset issuers to add complex rule sets that combine logical sets of rules that their asset must remain compliant with.

A rule can specify that a particular claim (issued by a trusted claim issuer) must exist, or must not exist, or that a group of claims must be present, or not present. It can also enforce that the sender or receiver is a particular identity.

```
pub enum ConditionType {
    /// Condition to ensure that claim filter produces one claim.
    IsPresent(Claim),
    /// Condition to ensure that claim filter produces an empty list.
    IsAbsent(Claim),
    /// Condition to ensure that at least one claim is fetched when filter is applied.
    IsAnyOf(Vec<Claim>),
    /// Condition to ensure that at none of claims is fetched when filter is applied.
    IsNoneOf(Vec<Claim>),
    /// Condition to ensure that the sender/receiver is a particular identity or primary issuance agent
    IsIdentity(TargetIdentity),
    /// Condition to ensure that the target identity has a valid `InvestorZKProof` claim for the given
    /// ticker.
    HasValidProofOfInvestor(Ticker),
}
```

## Transfer Restriction Smart Extensions

In addition to the flexible claim based compliance manager, asset issuers can use transfer restriction [smart extensions](./smart_extensions.md) to further control the trading of their assets.