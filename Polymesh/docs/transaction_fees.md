# Transaction Fees

Every on-chain transaction in Polymesh must be paid for using POLYX. The cost of the operation is proportional to the computational and storage complexity of the action being performed and is set directly by the network.

In order to execute an on-chain transaction, a user must have sufficient POLYX associated with the key which is signing and submitting the transaction. If there isn't a sufficient balance, the network will fail the transaction.

Transaction fees are split between operators and the network treasury, in a 20 / 80 ratio. This means that operators are incentivised to help increase adoption of the network (as they receive a percentage of transaction fees from on-chain activity) whilst allowing the treasury to grow its funds which can then be disbursed via the goverance process as grants to further increase the adoption or development of the network.

All transactions in Polymesh have a gas fee associated with them. The magnitude of the fee is determined by the compute / memory cost of executing the transaction, and the size (in bytes) of the transaction input. The gas fee is paid regardless of whether the transaction is successful or not.