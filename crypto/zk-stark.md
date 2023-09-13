# Zero Knowledge Proofs


## Chain State Proofs

1. Header chain proof
- Merkle Proof over the headers
- Hash Chain, PoW, difficulty adjustments

2. Assumevalid Proof
- All consensus rules except for script validation
- Manages the UTXO using [Utreexo](./utreexo.md)

3. Full Chain Proof
- Almost all consensus rules

## zk-STARKs

Better than SNARKs because they don't require a trusted setup/third party. Conservative cryptographic assumptions. Faster proof systems. 

1MB proof for 500GB chain

## For Bitcoin? 

No forks Prove once, millions can verify. 
Nodes can reshare proof. Anyone can be a prover & extend previous state proof.

## Header Proofs

Proof Recursion (looks like a Merkle tree), but you aggregate via proofs instead of hashes.

## Longest Chain Rule

Hard to prove something ( a longer chain ) doesn't exist. 
Honest peer assumption required. 
Resolve instantly


## Complexity

Very Complex
