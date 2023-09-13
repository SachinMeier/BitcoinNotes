# Why do we need HTLC-Success/Timeout transactions?

When Alice offers an HTLC to Bob, she creates a commitment transaction with an HTLC output. If Bob can provide the preimage within the HTLC Timeout, he can claim the coins. If he can't, Alice can reclaim the coins, but can only claim them after an absolute timeout (the HTLC's timeout) and a relative timeout (the commitment transaction's to_self_delay), which allows Bob to reclaim the coins if Alice is broadcasting old state. 

The way this works is that Alice sets up an HTLC like so (simplified):

```code
IF
    OP_HASH <hash> OP_EQUALVERIFY
    <bob_pubkey> OP_CHECKSIG
ELSE IF
    <cltv_expiry> OP_CHECKLOCKTIMEVERIFY OP_DROP
    <to_self_delay> OP_CHECKSEQUENCEVERIFY OP_DROP
    <alice_pubkey> OP_CHECKSIG
END IF
```

## HTLC-Success Transaction

## HTLC-Timeout Transaction

The problem with this simple setup is that Bob can provide the preimage at any point, even after the HTLC has expired (The CLTV has expired), if Alice hasn't yet executed the second branch due to the CSV relative timelock. This is a problem because Alice may have already broadcast the commitment transaction, and Bob can claim the coins from the HTLC output by CPFPing the his transaction.

Nevertheless, Alice can't just add a timelock to the successful branch of the HTLC output, because then Bob can't claim the coins if he has the preimage. 

The solution is to add a HTLC-Timeout transaction and move the `to_self_delay` into that transaction. 

```code
IF 
    OP_HASH <hash> OP_EQUALVERIFY
    <bob_pubkey> OP_CHECKSIG
ELSE IF
    <cltv_expiry> OP_CHECKLOCKTIMEVERIFY OP_DROP
    <htlc_timeoutpubkey> OP_CHECKSIG
END IF
```
