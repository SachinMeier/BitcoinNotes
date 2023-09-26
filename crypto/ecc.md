# Elliptic Curve Cryptography

## ECDSA


## Schnorr

MuSig2

P = p * G
A = a * G

B = P + A

B = (p + a) * G

sp = k1 + p * m
sa = k2 + a * m

sb = sp + sa

sb = (k1 + k2) + (b * m)

(Rb, sb)

## Nonce Reuse

in ECDSA or Schnorr, if you reuse a nonce, you leak your private key.

```math
s1 = k1 + x * m1
s2 = k1 + x * m2

m1, m2, s1, s2 are all public. 

s1 - s2 = (x * m1 + k1)
s1 - s2 = (k1 + x * m1) - (k1 + x * m2)
s1 - s2 = k1 + (x * m1) - k1 - (x * m2)
# k1 cancels out!
s1 - s2 = x * m1 - x * m2
s1 - s2 = x * (m1 - m2)
x = (s1 - s2) / (m1 - m2)
```

If a signature/public key has the same R value in an (R,s) signature AND the same Public key, the nonce has been reused and you can 