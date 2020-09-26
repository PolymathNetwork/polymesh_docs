## Transaction Fees

Every on-chain transaction in Polymesh must be paid for using POLYX. The cost of the operation is proportional to the computational and storage complexity of the action being performed and is set directly by the network.

In order to execute an on-chain transaction, a user must have sufficient POLYX associated with the key which is signing and submitting the transaction. If there isn't a sufficient balance, the network will fail the transaction.

Transaction fees are split between operators and the network treasury, in a 20 / 80 ratio. This means that operators are incentivised to help increase adoption of the network (as they receive a percentage of transaction fees from on-chain activity) whilst allowing the treasury to grow its funds which can then be disbursed via the goverance process as grants to further increase the adoption or development of the network.

All transactions in Polymesh have a gas fee associated with them. The magnitude of the fee is determined by the compute / memory cost of executing the transaction, and the size (in bytes) of the transaction input. The gas fee is paid regardless of whether the transaction is successful or not.

## Protocol Fees

In addition to transaction fees, certain on-chain transactions carry additional fixed fees.

The transactions that carry these additional protocol fees are configurable and can be updated via the governance process - modifying both the transactions that carry these fees, as well as the fee amounts for each transaction.

Currently only two types of transactions carry additional fees - these are:  
- registering a new ticker - this has a 2,500 POLYX fee  
- creating a new asset - this has a 10,000 POLYX fee  

Protocol fees are also split between operators and the treasury, using the same 20 / 80 ratio. The operator portion of both transaction and protocol fees are paid to the operator that produces a block that includes the relevant transactions.

Protocol fees are only paid by a user if their action is successful - for example if you try and register a ticker that has already been registered, you won't be charged the 2,500 POLYX protocol fee.

## Network Treasury

Polymesh has a special account, designated as the network treasury. This account collects a percentage of all fees and can also accept direct donations.

POLYX can only be disbursed from the treasury using the governance process, in particular by the submission and ratification of a PIP which represents a disbursement from the treasury to a specified acccount.

The intention is that treasury funds are disbursed to facilitate the adoption and development of Polymesh - it may be used for grants, direct payments to identities who perform specified actions for the network or any other purpose that the governance process deems reasonable.