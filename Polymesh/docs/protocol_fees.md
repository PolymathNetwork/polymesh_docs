# Protocol Fees

In addition to transaction fees, certain on-chain transactions carry additional fixed fees.

The transactions that carry these additional protocol fees are configurable and can be updated via the governance process - modifying both the transactions that carry these fees, as well as the fee amounts for each transaction.

Currently only two types of transactions carry additional fees - these are:  
- registering a new ticker - this has a 2,500 POLYX fee  
- creating a new asset - this has a 10,000 POLYX fee  

Protocol fees are also split between operators and the treasury, using the same 20 / 80 ratio. The operator portion of both transaction and protocol fees are paid to the operator that produces a block that includes the relevant transactions.

Protocol fees are only paid by a user if their action is successful - for example if you try and register a ticker that has already been registered, you won't be charged the 2,500 POLYX protocol fee.