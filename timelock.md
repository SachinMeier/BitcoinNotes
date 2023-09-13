# TimeLocks

2 types of timelocks are available:
- Transaction Timelocks: Make a transaction unspendable until a certain block height
- Script Timelocks: Make a script unspendable until a transaction timelock is unlocked

## Median Time Past

MTP is the median of the last 11 blocks' timestamps. It is used to calculate the minimum required timestamp for block. This prevents miners from manipulating timestamps and from the network moving backwards in time. 

## Transaction Timelocks

Timelocks can be relative or absolute. 

Timelocks are specified in block height or Unix timestamp. They are specified in the `locktime` field of a transaction.

If a `locktime` value is greater than 500 million, it is interpreted as a Unix timestamp. Otherwise, it is interpreted as a block height.

“Locktime” means absolute.

“Sequence” means relative.

Script-level time locks need a transaction-level lock of the same type in the spending tx.


### Relative Timelocks

Relative Timelocks prevent a transaction from being valid until a certain amount of time has passed since its inputs were created (the previous transaction was mined).

Relative locks can be set on un-confirmed transactions


### Absolute Timelocks

Absolute timelocks lock a transaction as invalid until a certain block height or timestamp is reached.

Lightning transactinos use CLTV (CheckLockTimeVerify) to apply absolute locktimes to HTLCs because the funding transaction for several channels in a route will have been confirmed for different amounts of time when the HTLC is created and we want explicit expiry times for a payment because in a route, HTLCs should expire in a specific order to keep all forwarding parties safe. 

If alice routes to Charlie via Bob, and Alice's HTLC to Bob expires before Bob's HTLC to Charlie, Alice can claim the HTLC via timeout while Charlie can claim the HTLC via preimage, leaving Bob at a loss. 


## Sources

https://medium.com/summa-technology/bitcoins-time-locks-27e0c362d7a1

Blocks
100 -------------------------> 200

Tx1                             Tx2
Output 1 --------------------> Input 1 
                                sequence = 100