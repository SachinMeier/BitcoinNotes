# Hashed Timelock Contracts (HTLCs)

An HTLC is an output script for bitcoin transactions that allows you to lock up coins until a certain time or block heightand requires providing a secret whose hash is specified in the contract.

HTLCs are used for Lightning Network payment channels and atomic swaps.

## Lightning HTLCs

In Lightning, channels are comprised of 2-of-2 multisig outputs of a funding transaction. Alice and Bob then each update the commitment transaction, which spends this funding transaction. The commitment transaction has 2 outputs: one for Alice and one for Bob. When Alice wants to send a payment to or through Bob, she adds another output to an updated version of the commitment transaction, an HTLC output. 

Each party actually keeps a different version of this newest commitment transaction. 

Any output in an HTLC that goes to the party holding that version of the HTLC should have a `to_self_delay` relative timelock, preventing it from being spent for a certain amount of time. This is to prevent the other party from broadcasting an old state and claiming the HTLC output.

### Alice (sender's) Commitment Transaction with HTLC

Simplified Lightning HTLC: 

```
IF 
- Bob Signature && Preimage # If bob knows the secret, he can claim the coins
ELSE IF
- Alice Signature && OP_CLTV(cltv_expiry) # After an absolute timeout, Alice can claim the coins. BUT these coins go to an HTLC Timeout transaction, not Alice. The HTLC Timeout transaction has a relative timeout, so that Bob cannot claim the HTLC after the OP_CSV relative timelock and before the OP_CLTV absolute timelock. The HTLC Timeout transaction ALSO has a revocation path so that Bob can claim the coins if Alice broadcasts an old state.
ELSE IF
- Bob has the revocation key # If Alice broadcasts an old state, Bob can claim the coins
ENDIF
```

[Example Alice Offers Bob an HTLC](https://ellemouton.com/lnThings/day7_2.png#center)

In Script, Alice's Commitment Transaction HTLC ScriptPubKey looks like this: 


```code
# To Bob's pubkey hash if he knows Alice's revocation key (no timelock)
OP_DUP OP_HASH160 <alice_revocation_key> OP_EQUAL
OP_IF
    OP_CHECKSIG
OP_ELSE
    <remote_htlcpubkey (bob's)> OP_SWAP OP_SIZE 32 OP_EQUAL
    OP_NOTIF
        # to node via HTLC-timeout transaction (a 2-of-2 multisig) (timelocked).
        OP_DROP 2 OP_SWAP <local_htlcpubkey (alice)> 2 OP_CHECKMULTISIG
    # If Bob knows the preimage, he can claim the coins (no timelock)
    OP_ELSE
        # to remote node with preimage.
        OP_HASH160 <RIPEMD160(SHA256(preimage))> OP_EQUALVERIFY
        OP_CHECKSIG
    OP_ENDIF
OP_ENDIF
```

The HTLC-timeout transaction scriptpubkey looks like this: 

Alice's HTLC-timeout Transaction has `nLocktime` specified to the CLTV delta.

```code
OP_IF
    # penalty transaction, no timeout
    <alice_revocation_key>
OP_ELSE
    # Alice can claim after the relative timeout
    <to_self_delay>
    OP_CHECKSEQUENCEVERIFY
    OP_DROP
    <alice_delayed_pubkey>
```

### Bob (receiver/forwarder)'s Commitment Transaction with HTLC

Bob's version of the commitment transaction has a different scriptpubkey for the HTLC output. 

if bob has the preimage, there should be a relative `to_self_delay` timelock, but to avoid the case where the `to_self_delay` relative timelock is longer than Alice's absolute timelock, This script path sends the coins to an HTLC Success transaction, which has the relative timelock. 

If alice has bob's revocation key, she can take the HTLC output immediately

If Bob fails to provide the preimage within the timeout, Alice can claim the coins. (CLTV Delta)


Bob's HTLC commitment transaction is NOT timelocked.

```code
# To remote node with revocation key
OP_DUP OP_HASH160 <RIPEMD160(SHA256(revocationpubkey))> OP_EQUAL
OP_IF
    OP_CHECKSIG
OP_ELSE
    <remote_htlcpubkey> OP_SWAP OP_SIZE 32 OP_EQUAL
    OP_IF
        # To local node via HTLC-success transaction.
        OP_HASH160 <RIPEMD160(payment_hash)> OP_EQUALVERIFY
        2 OP_SWAP <local_htlcpubkey> 2 OP_CHECKMULTISIG
    OP_ELSE
        # To remote node after timeout.
        OP_DROP <cltv_expiry> OP_CHECKLOCKTIMEVERIFY OP_DROP
        OP_CHECKSIG
    OP_ENDIF
OP_ENDIF
```


Stack:

<remote_htlc_pubkey>
<alice_htlc_sig>



ScriptSig:



IF revoking:
- Rev Pubkey
- Rev Signature

IF Timeout:
- 0
- Alice HTLC sig 

IF Success:
    0 <remotehtlcsig> <localhtlcsig>  <payment_preimage>

[Bob's HTLC Commitment Transaction](https://ellemouton.com/lnThings/day7_4.png#center)


## Sources

[Elle Blog Post](https://ellemouton.com/posts/htlc-deep-dive/)