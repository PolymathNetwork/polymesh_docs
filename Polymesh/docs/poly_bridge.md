# Ethereum POLY to Polymesh Bridge

## Overview

Since the Polymath team committed to building a securities blockchain on Substrate, development has shifted focus away from Ethereum. To support existing and future Poly holders, the team has created the bridge. At the time of writing the bridge lets holders of Kovan POLY tokens carry those tokens over to our brand new Aldebaran testnet. When the Polymesh mainnet launches, it is going to be bridged with the Ethereum mainnet.

Carrying the tokens over follows the standard *lock and mint* process. First, Ethereum tokens are locked indefinitely by the Ethereum smart contract. Then, native tokens are minted on the Polymesh side.

## Bridge Architecture

### Software

The bridge is a distributed system made up of the Ethereum smart contract, off-chain relay middleware and a dedicated Polymesh runtime module aka [*bridge pallet*](https://docs.polymesh.live/polymesh_runtime_common/bridge/index.html).

#### Ethereum Side

Any POLY holder can lock their POLY. The smart contract only locks POLY and rejects any other tokens. In order to be locked, the amount must be greater than 1 POLY. There is no maximum on how much POLY can be locked. Locked POLY stays locked forever and no-one can unlock it. The granularity for locked POLY is restricted to that of the Polymesh chain, that is, 6 digits after the decimal period.

The user provides their Polymesh address when locking POLY, however the contract does not check the validity of that address. The user must sign their POLY lock request with their Polymesh account private key, which ensures that they control that Polymesh account because that signature can be verified against the public key of the Polymesh account. Once the POLY is locked, the contract emits an event with the locked amount, the destination Mesh address and the lock transaction hash.

The contract allows meta-transactions so that third parties such as exchanges can act on users' behalf. Lastly, the smart contract is upgradable.

#### Relayer Middleware

Lock events emitted by the Ethereum POLY locker contract are listened to by *relayer* processes and verified by a sufficient amount of block confirmations. Each relayer process extracts Ethereum lock transactions from the received events and requests a connected *signer* process to sign those transactions before proposing thus signed transactions to the on-chain Polymesh bridge multisig, more details below.

Each signer has a Polymesh address which is registered in the bridge multisig wallet. This way the signers act as multisig voters. However the signers' role is only to passively sign the incoming transactions. It is the responsibility of the relayers to submit the signers' votes to the Polymesh chain.

#### Polymesh Side

Minting of POLYX tokens is implemented in the Polymesh runtime, in the bridge pallet. Below is the state machine diagram for the lifecycle of a bridge transaction arriving at Polymesh from the bridge relayers. There are 5 states: absent, timelocked, pending, handled and frozen. Any successful transaction makes its way from absent (start) to handled (end). The solid arrows correspond to automated transitions according to the business logic. The dashed arrows correspond to manual actions of the bridge administrator. The administrator has the ability to freeze a transaction that is still in progress. This is however an exceptional measure only to allow for the human to stay in the loop. It is supposed to be exercised only in extreme cases.

![](https://i.imgur.com/i7tSqI0.png)

Relayers are members of the bridge multisig account owned by the *bridge controller*. On the testnet, the number of relayers at genesis is 5. To start handling a transaction -- that is to change its status from absent -- at least 3 out of 5 relayers have to propose that same transaction.

Even though there is no maximum limit of how much POLY can be bridged, the Aldebaran testnet side sets a limit on how much POLY is bridged per unit of time. There is going to be no such a limit on the mainnet.

### Cluster

The bridge is a set of *bridge clusters*. Each of those clusters contains the following components:
* a public Internet-facing Ethereum node,
* a public Internet-facing, non-validating Polymesh node,
* a private relayer node behind a private connection to the Ethereum and Polymesh nodes,
* a private signer privately connected to the relayer.

## UI Walkthrough

Polymath has created a comprehensive state-of-the-art and easy to use [bridge UI](https://polybridge.polymesh.live/).

![](https://i.imgur.com/Ic5lw94.png)

connect with your Metamask account:

![](https://i.imgur.com/5w1O80f.png)

enter the recipient POLYX account:

![](https://i.imgur.com/V5QsHPU.png)

The UI detects whether the recipient account is linked with an identity. If it isn't, a new identity can be created:

![](https://i.imgur.com/QhT7Azb.png)

Just follow the verification prompts and you should get a successful identity verification:

![](https://i.imgur.com/7y2EEsv.png)

After that bridging POLY is straightforward -- simply tell the UI how much should be bridged:

![](https://i.imgur.com/L2PzsLl.png)

Hit "Confirm" and -- while the timelock is on -- grab a cup of tea or have a look at other Polymath products like the [Token Studio](https://tokenstudio.polymesh.live/).

**Happy POLY bridging!**
