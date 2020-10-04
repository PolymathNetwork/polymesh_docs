## Overview

POLYX is the native token of the Polymesh network.

It is used in Polymesh for staking, for both operators and non-operators, signalling on governance issues and paying for transactions on the network.

Polymath has an ERC20 token on the Ethereum mainnet, POLY, used for our Ethereum ST20 protocol. POLY ERC20 tokens can be swapped one-to-one for POLYX tokens on Polymesh via the [bridge process](./bridge.md).

Additional POLYX is only created when block rewards are minted in order to reward operators and stakers.

Any Polymesh key can hold POLYX, but it can only be sent and received by a key which is associated with an identity that has a valid Customer Due Diligence claim.

POLYX can be [staked](./tokenomics.md) in order to receive a proportional amount of the block reward assigned to stakers. Operators are also required to stake a minimum bond in order to run an authoring node.

## Security

Nodes which participate in the [consensus](./consensus.md) mechanism of Polymesh and produce new blocks, must post a POLYX bond. This bond is at risk if the operator fails to behave correctly, for example being offline, or producing inconsistent consensus messages. This provides security to the network by incentivising operators to ensure the network remains live and finalises blocks rapidly.