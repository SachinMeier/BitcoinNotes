# Taproot

An upgrade to Bitcoin to add Schnorr Signatures, new opcodes, and a new ScriptPubKey format. Taproot adds Pay-to-Taproot (P2TR), which is a new ScriptPubKey format that allows for a single private key to be used to spend the coin, but also allows for more complex spending conditions to be used, through MAST. 

## TapTweak (Tweaking the Taproot spending key)

You have a public key `P` made from a private key `p * G`. You also have a Merkle Tree of alternative scripts that can be used to unlock the coin. You take the root of the Merkle Tree `h` and tweak `P` with `h * G` to get a new public key `Q`. `Q` is the external Taproot key seen in the ScriptPubKey.

```
P' = P + h * G
   = p * G + h * G
   = (p + h) * G
```

## KeySpend 

If you want to spend the coin using only the single private key `p`, you must tweak the private key `p` with the Merkle Root of the script tree `h` to get a new private key `q`. `q` is the external Taproot private key used to spend the coin. `q` is the private key that corresponds to the public key `Q` in the ScriptPubKey. This means that a KeySpend will look exactly like a single signature from a single private key, hiding the fact that there may or may not have been alternative scripts that could have been used to spend the coin.


## Taproot X Lightning

Lightning uses 2-of-2 multisig scripts. Using SegWit, these 2-of-2 use P2WSH script types. With Taproot, these 2-of-2 multisig scripts can be replaced with a single key, making the channel open and close transactions look like single signature transactions. This means that Lightning channels can be opened and closed with a single signature, making them indistinguishable from normal transactions.

Only Lightning really uses 2-of-2, so currently, it's obvious what is a Lightning Channel (especially once the channel closes). Taproot will make Lightning channels look like normal transactions, making them indistinguishable from normal transactions.

Execption! Force closes will still be more recognizable since script path spending must be used. But this is still an improvement over the current situation.

Benefit of Old Lightning: 

- Peers can sign updates and messages independently, because they don't need to communicate and share nonces and then combine signatures. They just sign on their own and send that sig over. 

MuSig2:
- inputs from both parties are needed to produce a signature. 
- RISKS:
   - adversarial party sends bad nonce? 
   - Negligent party reuses nonce

Lightning is an asymmetric protocol: 
   - need signatures for ourselves (local commitment)
   - need signatures for the other party (remote commitment)