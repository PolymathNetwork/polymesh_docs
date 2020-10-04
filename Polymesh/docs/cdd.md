## Overview

Polymesh has a unique on-boarding Customer Due Dilgence (CDD) process that leverages a federation of professional KYC companies to quickly and easily on-board users, whilst providing the network and its participants stronger security. Given the domain of Polymesh is regulated assets and capital markets, this allows investors, institutions and other users to interact with confidence on Polymesh, strengthing its value and aiding ecosystem adoption.

Every transaction that is executed on-chain in Polymesh must be associated with an identity that has a valid CDD claim. To be valid a CDD claim must be issued by one of a group of permissioned identities representing professional KYC companies.

To on-board into Polymesh, a user must go through a simple due diligence process with a Polymesh CDD service provider - that CDD service provider will then create the users identity and attach a CDD claim.

These checks for a valid CDD claim are performed as part of the networks pre-processing checks. This ensures that a transaction without a valid CDD claim cannot even pay a network fee for a failed transaction - they have no ability to interact with the network, including paying fees. This gives operators the confidence that all on-chain transactions are associated with a CDD'ed identity, including failed transactions.

## CDD Service Providers

CDD Service Providers in Polymesh are permissioned. This means that if an organisation wants to offer Polymesh users a CDD service, they must be approved through the on-chain governance process. A Polymesh Improvement Proposal would be created to add the new CDD service provider to the list of permissioned CDD service providers on-chain.

Similarily to remove an existing CDD service provider, on-chain governance would enact a proposal to remove their identity from the on-chain list of permissioned CDD service providers. When an existing CDD service provider is removed, the governance process can set a "sunset" window, during which their previously issued CDD claims are valid, but they cannot issue new ones. This allows users to receive a CDD claim from an alternative CDD service provider, and not lose access to the network.

## CDD Claims

CDD claims are issued by the networks trusted CDD service providers. They would typically have a 6 month expiry, meaning customers need to re-authenticate with the CDD service provider every 6 months to maintain access to the network.

An identity must always have a valid CDD claim if it wishes to be able to interact with the network. It may have more than one valid CDD claim, for example if they have KYC'ed with two separate CDD service providers. This provides some additional redundancy if a CDD service provider is removed from the trusted list of providers.

A user can also have multiple identities. In this case each identity must have a valid CDD claim, although these could be issued by the same CDD service provider meaning the user only needs to KYC once. These identities (from the same entity) are tied together using Polymesh Confidential Identity Mechanism - see the next section for details.