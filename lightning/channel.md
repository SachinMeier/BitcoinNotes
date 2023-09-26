# Lightning Channels


A Lightning Channel is a 2-of-2 multisig funded by a funding transaction which locks a single UTXO to the channel. The UTXO is then "spent" by unconfirmed "commitment" transactions which both parties update to reflect changes in their balance. 


Each party in the 2-of-2 holds a different version of the commitment transaction, so that their own local copy can have an additional timelock on their `to_local` output while the `to_remote` output remains un-timelocked. 

## Local Output

funding UTXO ----commitment transaction----> to_local output
                                       \
                                        \----> to_remote output

to_local Script: 

```code
OP_IF
    <revocationpubkey>
OP_ELSE
    <to_self_delay>
    OP_CSV
    OP_DROP
    <local_delayedpubkey>
OP_ENDIF
OP_CHECKSIG
```

ScriptWitness: 

if revoking: 

<revocation_sig> 1

if spending:

<local_delayed_sig> 0

To Remote script: P2WPKH


## Creating a Channel


A Lightning channel is a 2-of-2 multisig: 
`<pubkeyA> <pubkeyB> 2 OP_CHECKMULTISIG`

SoonTM, Taproot channels can simplify this to a single pubkey that represents both parties. 


BOLT2 defines the messaging for channel creation. 

1. Alice sends `open_channel` message to Bob, including a `funding_pubkey` field.
2. Bob decides he wants to accept, so sends `accept_channel` message which contains his `funding_pubkey`. Alice and Bob now both knokw the funding script. 
3. Alice needs to get Bob's signature of a commitment transaction before she can broadcast the funding transaction and open the channel (in case Bob disappears). But she first sends a `funding_created` message including `txid:vout` of the unsigned funding transaction to Bob and a signature for Bob's commitment transaction. Bob has previously shared all necessary data (output scripts, revocation pubkeys, etc.) with Alice so she can construct and sign Bob's version of the commitment transaction. 
4. Once Bob has a signed commitment transaction, he sends a `funding_signed` message to Alice with his signature of her commitment transaction. Now that Alice knows she can recover her funds, she can safely sign the funding transaction and broadcast it. 
5. Now both parties watch the blockchain for the funding transaction to confirm. Once it does, they each send eachother `channel_ready` msgs. 

[communication model](https://ellemouton.com/lnThings/open_chan_msg.png#center)

## Sources

https://ellemouton.com/posts/creating-a-channel/

