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


