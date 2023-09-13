# Zero Knowledge Proofs

Interactive Proofs are probabilistic. 

Zero Knowledge Proofs are a conversation between a prover and a verifier, which is convincing of a fact, and which is "Zero Knowledge". 

__Convincing__: A dishonest prover MAY fail to convince a verifier && and honest prover CANNOT fail to convince a verifier. 
    - Since the dishonest prover MAY fail, iteratively running the protocol will eventually convince the verifier.

__Zero Knowledge__: The verifier learns nothing useful except that the statement is true.

## Example: Coin Permutation

`c1` & `c2` are two coins. `b` is a bit. Prover will prove they know the difference between `c1` and `c2` without revealing which is which.

Prover & Verifier

1. Verifier determines the order of the coins, `π` is a permutation function
    - `π(c1) = cb`
    - Possibilities: `π(c1, c2) = (c2, c1)` OR `(c1, c2)`

2. Verifier sends challenge `π(c1)` to Prover. 

3. Prover returns the "guess" of the permutation, `b'`. `b'` is the guess of whether c1 was first or second. 

4. Verifier checks that `b' == b`


## Example: Are 2 Graphs Isomorphisms

### Prove that 2 graphs are NOT isomorphisms.

There are 2 graphs `M1` and `M2`. No permutation of `M1` is `M2`.

1. Verifier picks bit `b`, permutes `Mb` and sends it to Prover.

2. Prover returns `b'`, which is their guess of `b`.
    - Prover brute forces (or knows something about the difference between the graphs)

3. Verifier checks that `b' == b`.

### Prove that 2 graphs ARE isomorphisms.

There are 2 graphs `M1` and `M2`. There is a permutation of `M1` that is `M2`.

1. Prover picks a permutation `M3 = F(M1)` of `M1` and sends `M3` to Verifier. `F` is the permutation function.

2. Verifier picks a bit `b` and sends `b` to Prover.

3. Prover returns response `R`, which is `F` if `b == 1` or `F·π` (product of `F` and `π`) if `b == 2`. 
    Either way, `R(Mb) = M3`. 

!! IF the verifier can simulate a conversation with the prover, then the prover is safely not revealing any information.

### Schnorr Identification Protocol

Prover has key pair `X = x * G`

1. Prover generates nonce `k` and sends public nonce `R = k * G` to Verifier

2. Verifier sends challenge `e` to Prover

3. Prover sends response `s = k + x * e` to Verifier
    - s * G = R + (X * c)

4. Verifier checks that `R = s * G - (X * e)`

Transacript: (R, e, s)
Verifier can simulate by generating `e` and `s` randomly, and then compute `R = s * G - (X * e)`
Thus: No new information!

Schnorr is a ZKP that Prover knows the private key to the public key `X`.

### Schnorr Signature Protocol

(non-interactive, and has a message)

Instead of Verifier providing `e`, it is computed as `e = hash(R, X, m)`

`X` not strictly necessary, but a good idea to include the full context in the hash. Bitcoin does it. 


POTS
- STOP
- TOPS
- SPOT
- POST
- OPTS
- POTS
- TOP
- OPT
- POT
- SOT
- SOP
PEAR
- PEAR
- REAP
- PARE
- RAPE
- EAR
- PEA
- REP
- ARE
- ERA
- PER
- PAR
- APE
- RAP
STAR
- STAR
- RATS
- TSAR
- ARTS
- TARS
- TAR
- SAT
- RAT
- ART
EATS
- SATE
- SEAT
- EATS
- TASE
- TEAS
- EAST
- EAT
- ATE
- TEA
- SEA
- SAT
- SET
PAST
- PAST
- PATS
- SPAT
- TAPS
- ASP
- APT
- SAP
- SAT
- TAP
- PAT
- SPA