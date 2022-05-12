## Deadlock, lock contention
assuming a set of keys `{A, B, C}` and an arbitrary number of updates of each:
with reshuffle: each key will deterministically be assigned to a single partition
(only one writer will acquire locks for the key, no lock contention)
**without reshuffle**:
Each key will be assigned to a random partition (many writers will acquire locks for the key,
causing lock contention and possibly deadlocks) deadlocks can be avoided without reshuffle
if locks are acquired in the same sequence by all writers this won’t eliminate lock contention (reshuffle will), but it will eliminate deadlocks
example…

writer A receives updates for `[A, C]`  and locks them in order
writer B receives updates for `[C, B, A]` and locks them in order

if those writes happen concurrently it will cause a deadlock
if instead the records are sorted:
writer A: `[A, C]`
writer B: `[A, B, C]`
no deadlock will occur, instead we have lock contention. whichever writer acquires a lock on `A`
first will continue running until committing the transaction, blocking the other writer until the 
transaction is committed another thing to investigate is the bundle sizes when using Reshuffle, it’s 
certainly possible that reshuffling by the primary key is causing bundle sizes to be small, which would create a lot
of transactions with small batch sizes if this is the case, try reshuffling by smaller range of values… like `hash(key) % 16`
instead of the key itself, this will still ensure all updates to a key happen in the same writer.
however, I’m not sure if Reshuffle impacts bundle size (just a theory) and I don’t think the distinction between gap
and row locks matter at all since locks are on single keys, they should behave exactly like a row lock