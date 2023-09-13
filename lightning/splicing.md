# Channel Splicing

Increase or decrease the capacity of a channel without closing it.

Uses interactive-tx protocol

1. Alice and Bob agree on a capacity of 1 BTC for a channel. 

2. They want to increase the capacity to 2 BTC.

3. Double spend the Funding tx output (replacing the most up-to-date commit tx). This new tx is the Splice TX.
    - The Splice TX can also pay out to shrink the channel size. If the splice tx adds new funds and pays out, it can rebalance a channel if one peer adds and the other removes

4. This new Splice TX has as inputs, the channel point and the additional inputs to be added. 

5. The Splice TX is broadcasted. While it is unconfirmed, Alice and Bob must maintain the channel state updates for both the original commitment tx and the splice tx, so new HTLCs will be added to both. 

6. Once the Splice TX is confirmed, the old channel's latest commit tx can be safely dropped and all previous revocation keys from before the splice tx started being updated can be forgotten.