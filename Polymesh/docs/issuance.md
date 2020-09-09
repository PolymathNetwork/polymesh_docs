# Issuance

The issuance process for assets in Polymesh allows the originator of an asset (the asset issuer) to issue and distribute their asset to investors.

# Roles

Every asset, identified by its unique ticker, is associated with two identities:  
 - the asset issuers identity: this is the identity that created the asset
 - the primary issuance agents identity: this is the identity responsible for distributing the asset to investors.
 
These identities can be the same, and the primary issuance agent identity is in fact defaulted to the asset issuers identity.

The asset issuer can set the primary issuance agent for their asset to any other identity, although the targetted identity must accept this role via a `AcceptPIA` authorisation. In the case where the primary issuance agent is the asset issuer, no authorisation is needed.

# Process

Once an asset issuer has created and configured their asset, they can then issue tokens representing ownership in the asset to the primary issuance agent.

The primary issuance agent can then distribute those asset tokens to investors directly or via an security token offering, using the settlement engine.

[TODO] link to distribution section of Settlement

1. register ticker (asset issuer)
2. create asset (asset issuer)
3. issue asset to PIA (asset issuer)
4. distribute asset from PIA to investors (PIA)

# Diagram

...

