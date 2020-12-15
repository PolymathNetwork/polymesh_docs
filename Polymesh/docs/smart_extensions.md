## Overview

Smart Extensions are smart contracts deployed on top of the Polymesh blockchain, that add certain types of functionality to Polymesh primitives.

In the first iteration of Polymesh, smart extensions are supported for additional transfer restrictions, that may not be possible using the embedded [claim](./compliance_manager.md) primitive.

Smart extensions are currently written in [Ink!](https://github.com/paritytech/ink) - a Rust based DSL for smart contracts on top of Substrate based chains like Polymesh. In the future Polymesh may introduce DSLs specific to the type of smart extension being authored.

A smart extension must conform to a particular interface determined by the type of the smart extension. This allows the corresponding Polymesh primitive to leverage a smart extension in a reliable and consistent fashion.

## Deploying Smart Extensions

Any Polymesh user, with a valid CDD claim, can deploy a smart extension template.

A smart extension template is a copy of the smart extensions logic held on-chain, from which specific instances of the smart extension can be created, and attached to a Polymesh primitive (for example an asset, in the case of a transfer restriction).

Various metadata about the smart extension is captured when it is deployed, including an instantion fee set by the creator of the template, which will be paid by users of the template (i.e. asset issuers that attach it to their asset for compliance purposes). A usage fee is also captured, although this is not currently used in-protocol. It may be used in the future with new types of smart extension.

## Transfer Restriction Smart Extensions

To use a transfer restriction smart extension it must first be deployed to the Polymesh blockchain as a template, with some additional metadata:  

```
/// Subset of the SE template metadata that is provided by the template owner.
#[derive(Encode, Decode, Default, Debug, Clone, PartialEq)]
pub struct TemplateMetadata<Balance> {
    /// Url that can contain the details about the template
    /// Ex- license, audit report.
    pub url: Option<MetaUrl>,
    /// Type of smart extension template.
    pub se_type: SmartExtensionType,
    /// Fee paid at the time of usage of the SE (A given operation performed).
    pub usage_fee: Balance,
    /// Description about the SE template.
    pub description: MetaDescription,
    /// Version of the template.
    pub version: MetaVersion,
}

/// Data structure that hold all the relevant metadata of the smart extension template.
#[derive(Encode, Decode, Default, Debug, Clone, PartialEq)]
pub struct TemplateDetails<Balance> {
    /// Fee paid at the time on creating new instance form the template.
    pub instantiation_fee: Balance,
    /// Owner of the SE template.
    pub owner: IdentityId,
    /// power button to switch on/off the instantiation from the template
    pub frozen: bool,
}
```

Once an asset issuer has created an instance of a transfer restriction smart extension and configured it, they can attach it to their asset using:  

```
//! - `add_extension` - used to permission the a smart extension address for a given ticker.
//! - `archive_extension` - used to archive a smart extension meaning the extension is not used to verify compliance
//! - `unarchive_extension` - used to un-archive a smart extension meaning the extension is now used to verify compliance
```

A transfer restriction smart extension must have a `verify_transfer` function. This is called by the asset primitive to verify that a settlement is compliant:  

```
        pub fn verify_transfer(
            &self,
            _from: Option<IdentityId>,
            _to: Option<IdentityId>,
            value: Balance,
            balance_from: Balance,
            balance_to: Balance,
            _total_supply: Balance,
            current_holder_count: Counter,
        ) -> RestrictionResult
```

## Smart Extension Fees

The author of a smart extension template can set a fee in POLYX that a user is required to pay to create an instance of that template for their own usage. This fee is split between the smart extension author, the network treasury, and network operators.

Whilst we capture a usage fee, this is not currently paid in-protocol as it isn't appropriate for transfer management smart extensions.

There is a protocol fee paid every time a smart contract template is deployed. The value of this fee is managed via on-chain governance.

In addition, the usual POLYX transaction fees apply.