# Multisig

## OP_CHECKMULTISIG

Bitcoin script includes an opcode for explicit multisig. It allows threshold signatures, but is not very flexible. 

Script: OP_CHECKMULTISIG <N> <keyA> <keyB> <keyC> ... <M>
Unlocking: 0 <sigA> <sigB> <sigC> ...

With Taproot, OP_CHECKMULTISIG was replaced by OP_CHECKSIGADD, which is more flexible. It is order-independent, so signatures can be added in any order. 

## MuSig2

MuSig was broken (proven insecure), so MuSig2 was created. MuSig2 is a simple way to do N-of-N multisig. It is not threshold signatures, like [FROST](#frost). 

Alice and Bob want to do MuSig:

1. Alice and Bob each generate a keypair.
- Alice: `a`, `A = a * G`
- Bob: `b`, `B = b * G`
- `P = A + B`
2. Alice makes 2 nonces
- `RA' = ra' * G`
- `RA'' = ra'' * G`
3. Bob makes 2 nonces
- `RB' = rb' * G`
- `RB'' = bb'' * G`

The Nonces are summed together:
`R = R' + R''`

WHERE `R' = RA' + RB'`
        `R'' = RA'' + RB''`

R is multiplied by 'nonce coefficient' `c`:

```
c = hash(R' || R'' || P || m)
```

GOAL: neither Alice nor Bob can make an 'adversarial nonce'

## FROST

FROST lets you do threshold signatures with a group of signers. i.e. M-of-N multisig. This is in contrast to [MuSig2](#musig2), which is simple multisig (N-of-N). 

FROST relies on [Shamir's Secret Sharing](./crypto/shamir-secret-sharing.md) to split the private key into shares.

## ROAST

???