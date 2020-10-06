## Overview

An _asset checkpoint_ is a collection of records representing the balances of that asset at a given
time. The balances recorded are the total asset balance and all the balances held by identities.


## Creating a checkpoint

There are two ways to create a checkpoint.

1. Call the dispatchable `create_checkpoint(ticker)` in the `asset` pallet. This creates a
   checkpoint for the asset of `ticker`. The total balance is recorded instantly, and each balance
   held an identity is recorded just before the next transaction to or from that identity. Since the
   balances of identities are not recorded instantly, we call this process _lazy checkpointing_. A
   consequence of this method is that no records are made if there are no transactions.

2. Create a checkpoint schedule using the dispatchable `create_checkpoint_schedule(ticker,
   schedule)` in the same pallet. A schedule consists of a starting timestamp in [seconds since the
   UNIX epoch][unix_time] and a period for recurring the checkpoint at regular intervals. The period
   is a multiple of one of seconds, minutes, hours, days, weeks, months, or years. If the period has
   zero length then the checkpoint schedule it defines is non-recurring and the only checkpoint it
   contains is the one at the start. Scheduled checkpoints are created automatically at times
   defined by the schedule but are otherwise identical in nature to manual checkpoints. Scheduling
   of the next checkpoint happens lazily as well, on an asset transaction or minting. If there are
   no such transactions by the time a checkpoint is due, it does not get rescheduled, which is fine
   with the scheduler since it takes into account any missed checkpoints.


### Checkpoint periods

There are two kinds of periods: ones of fixed length if measured in seconds -- multiples of seconds,
minutes, hours, days or weeks -- and ones of variable length -- multiples of months or years.

When computing the time when the next checkpoint is due, if the period is fixed the runtime adds the
length of that period in seconds to the time when the last checkpoint was due.

If the period is variable, the next timestamp computation takes into account the day of the month at
the start of the schedule. Every checkpoint gets a day of the month that is at most that of the
start of the schedule.

For example, assume we have the checkpoint schedule that

* starts on 00:01:00 the 31st of January, 2021 UTC and

* has a period of one month.

The checkpoint timestamps are going to be

* 2021-01-31 00:01:00

* 2021-02-28 00:01:00

* 2021-03-31 00:01:00

* 2020-04-30 00:01:00

and so on.


## Accessing an existing checkpoint

The checkpoints of an asset form a sequence that is indexed starting from 1. The total balance and
balances of identities are stored at these indices.

To read the total supply of `ticker` at checkpoint index `i` one calls the getter
`total_supply_at(ticker, i)` in the asset pallet. The getter returns the value directly from runtime
storage.

To read the checkpoint balance of `identity` one calls the function `get_balance_at(ticker,
identity, i)` in the same pallet. This function returns the checkpoint balance at `i` by searching
for the least checkpoint index greater or equal to `i` or, if no such record exists due to the
absence of transactions, the current asset balance of `identity`.

The time at which a checkpoint was made is stored in a map indexed by checkpoint sequence numbers
and can be read knowing the required sequence number.

[unix_time]: https://en.wikipedia.org/wiki/Unix_time
