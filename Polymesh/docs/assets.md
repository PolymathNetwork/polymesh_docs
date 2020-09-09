# Assets

Assets on Polymesh can represent any type of digitalised asset, and are originated and managed through the asset base layer logic in Polymesh.

This ensures that all assets are created in a standardised manner, allowing related functionality such as corporate actions, settlement and compliance, to function seamlessly across all assets.

Once an asset has been created, its ownership is represented via balances of that assets tokens held by different investors.

Ownership of an asset on Polymesh can be determined via its:
  - total supply: the total number of tokens that represent ownership in the asset
  - investor balances: the individual balance of each investing identity in the asset

The asset pallet documentation can be found at:  
https://docs.polymesh.live/pallet_asset/index.html

Polymesh allows you to manage the full lifecycle of any asset directly on the Polymesh blockchain, including the issuance, initial distribution or fundraise of the asset and any subsequent corporate actions such as dividend payments, capital distributions or corporate ballots.

Asset tokens can be either divisible or indivisible.

## Ticker Registration

Every asset on Polymesh has a unique ticker - a 12 character identifier for the asset.

Users can register a ticker, and then when they are ready create an asset associated with that registered ticker.

Ticker registrations can be transferred between identities and have an expiry date, currently set to 6 months from their initial registration.

Registering a ticker ensures no other user can issue an asset with that ticker whilst the registration is valid (i.e. has not expired).

## Asset Creation

Having registered a ticker, users can then create their asset, specifying the type of the asset (e.g. Equity, Bond, Fund).

The asset is initially created with a zero total supply. For the initial issuance of the asset, see the Issuance section [TODO link].

As well as being identified via a unique ticker, assets can be associated with additional external identifiers, such as ISINs, CUSIPs, CINs, LEIs and DTIs, with Polymesh validating that these identifiers are consistent (i.e. that their checksums match).

If a ticker is not currently reserved, the ticker registration is done automatically when a user creates an asset.

## Documentation

Polymesh allows the issuer of an asset to associate documents with that asset. These documents can include [TODO] XXX.

The actual document is not stored on-chain, and instead the asset is associated with a document reference on-chain, where the reference includes a name, the URL at which the document can be found (this may be a permissioned URL requiring an investor in the asset to have some credentials to access the document) as well as an optional hash of the document contents.

Documentation can only be updated or modified by the identity that issued the corresponding asset (the asset issuer).

## Ownership

All asset ownership in Polymesh is at the identity granularity. Whilst an investor can organise their assets into different portfolios under their identity [TODO link], compliance is enforced at the identity level, so in order to pay or receive a particular assets token, the relevant identity must have claims that match the compliance rules specified by the asset issuer for the asset. [TODO link]

## Asset Issuers

The asset issuer is the identity which registered the ticker and created the asset. The asset issuer can transfer ownership of an asset to another identity by issuing a `TransferAsset` authorisation which must be accepted by the target identity.

The asset issuer also has some additional controls which are soley accessible to them. These include:
  - updating documentation and identifiers for their asset
  - freeze and unfreeze any transfers of their asset
  - add and remove compliance rules for their asset
  - permission venues to settle their asset

In addition an asset issuer can execute a controller transfer of their token. This allows them to force transfer ownership of their asset tokens from any investor back to the primary issuance agent of the asset. The primary issuance agent is an identity which an asset issuer specifies for their asset, and is responsible for treasury management and token distribution. [TODO link]