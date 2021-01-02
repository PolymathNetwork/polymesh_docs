## Overview

POLYX is the native token of Polymesh and is used to stake and pay for transactions on the network.

POLYX provides security to the Polymesh network by:

- metering the cost of compute and storage through [transaction fees](./fees.md) preventing Denial of Service attacks

- incentivising correct behaviour by operators by rewarding them for acting correctly, and penalizing them for downtime or inconsistent data.

## POLY / POLYX Total Supply

Polymath has an ERC20 token on the Ethereum mainnet - [POLY](https://etherscan.io/token/0x9992ec3cf6a55b00978cddf2b27bc6882d88d1ec), with a fixed total supply of 1,000,000,000 POLY.

The POLY to POLYX bridge converts these ERC20 POLY tokens to POLYX on the Polymesh network on a one for one basis. Bridging POLY to POLYX does not effect the total supply.

In addition, to reward operators and stakers, the Polymesh network protocol mints rewards at the end of each era which are distributed pro-rata to operators and stakers.

## Reward Curve

The rewards paid to operators and stakers on Polymesh varies based on the amount of POLYX currently being used to stake.

The ideal staking ratio will be initialised to 70% (i.e. 70% of the total supply of POLYX on Polymesh being staked), but can be modified via governance.

Whilst the staking ratio is lower than this amount, rewards are relatively high to encourage new stakers. Once we reach the ideal staking ratio, rewards drop off exponentially encouraging additional liquidity (i.e. unstaking).

For more details see:  
<https://research.web3.foundation/en/latest/polkadot/economics/1-token-economics.html>

## Fees

Transaction fees provide Polymesh with a spam prevention mechanism, preventing Denial of Service style attacks. The cost of a transaction (denominated in POLYX) is determined via its computation and storage costs. This is done by benchmarking each extrinsic and setting the POLYX fee accordingly.

Polymesh processes transactions on a first in, first out basis. There is no equivalent to "gas price" (as seen in Ethereum) and tipping (in order to have your transaction included earlier) is not allowed in Polymesh.

## Rewards and Penalties

Polymesh has a fixed upper limit of operator nodes which are chosen in each era to participate in the consensus protocol (GRANDPA and BABE) and earn rewards for themselves, and their stakers.

If there are more candidate operator nodes, than available slots, the operator nodes with the most stake will be chosen. This encourages competition amongst operators to attract stake, which translates to reasonable commissions, and emphasises the need to have an excellent reputation and track record of stable performance, which in turn promotes the stability and security of the Polymesh network.

Each operator can run multiple nodes, although there is a fixed upper limit on the number of nodes that a single permissioned operator identity can run. This prevents an operator from being able to capture the whole network, even if they capture a majority of the networks stake.

Operators can set a commission, although there is a global cap on the commission that each individual operator can set.

Regardless of their stake, each chosen operator has an equal chance of receiving rewards during an era. Rewards during an era are tracked via points for certain actions (e.g. creating a block, voting correctly on a block) and at the end of an era, these points are used to proportionally divide the eras reward between operators.

Once a reward has been calculated for a particular operators, the commission is first removed and paid directly to the operator. The remaining reward is then split proportionally across all accounts that have staked that particular operator, including the operator themselves if they have self-staked.

If an operator is off-line, or equivocates (votes on two contradictory blocks) they can be fined. The fine depends on a number of parameters, including the overal state of the network (e.g. how many other nodes are also offline). For more details see:  
<https://research.web3.foundation/en/latest/polkadot/slashing/amounts.html>

## Treasury

A percentage of all transaction fees (80%) are accumulated in the network treasury account. This is a special account that can only be accessed through governance. The intention is that the network treasury will be used to pay out grants and rewards to further the development and adoption of the Polymesh network.