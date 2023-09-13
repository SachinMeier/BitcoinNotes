# Fiat Shamir Transform

Take an interactive protocol, use a protocol simulator (a hash function) to turn it into a non-interactive protocol.

Fiat Shamir doesn't work in every use case. 

### Schnorr Identification Protocol

$ is a random number 

A = a * G
k = $
R = k * G

1. Alice sends R
2. Bob (challenger) sends c = $
3. Alice sends s = k + a * e,  (R, s) is a signature
4. Bob verifies that R = s * G - c * A

### Schnorr Signature Protocol

A = a * G
k = $
R = k * G
e = hash(R, m)
s = k + a * e

1. Alice sends (R, s) (NO INTERACTIVITY)

