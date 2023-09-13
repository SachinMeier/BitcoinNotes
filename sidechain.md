# SideChain

## Merged Mining

Mining two blockchains at the same time.  The second blockchain is called the auxiliary blockchain.  The auxiliary blockchain is merged mined with the primary blockchain. 

Bitcoin blocks contain two hashes corresponding to 2 sets of transactions, the Merkle Root of 2 different transaction sets. The first one is the regular Bitcoin transaction set for that block. The second one is the auxiliary blockchain transaction set.

## 1-way Peg

Burn Bitcoin via OP_RETURN to get tokens on the auxiliary blockchain. 

Theory: No one will buy spacecoins if creating them is cheaper, so there's a price ceiling. 

## Blind Merge Mining

Do not restrict merged mining to only miners. Allow any OP_RETURN to be a merge mined transaction set (block)

## ANYONE_CAN_SPEND Model

One way of running a merge-mined sidechain to create a linked-list of transaction set root hashes (merge-mined blocks) is to dedicate a single output that always goes to a specified ACSA. "sidechain miners" move a bit of dust around and attach a hash of the next block to an OP_RETURN in that tx. 

- Put Dust amount of sats in an anyone-can-spend address (ACSA). 
- Look for a linked list of txs containing a hash and recreating the ACSA. 
- Thefts are detectable: linked list suddenly points to a new address
- if this happen, let anyone get the linked list going again by making a new ACSA output. 

## Prepaid Model

- Sponsors make an N-of-N multisig and crowdfund some sats. 
- Cosign a linked list of Blind Merge Mined txs
    - Leave the OP_RETURN 32 byte data field blank
    - Set relative timelock to space out blocks?
- Users trust 1+ of the original Sponsors
- At least one of the Sponsors deletes their keys
- Loop forever

## Spacechains: 2 way peg

- Use the PrePaid Model
- People only "peg in" at the beginning
- Amount that gets pegged in gradually "leaks" back to the mainchain as tx fees
- ONLY bitcoin miners can effectively "peg out"
- But now you've ruined the price cap

### Use Cases
- Destroy altcoins
- DNS replacement
- Stablecoins & NFTs
- other 2-way pegs

## Covenants

A [covenant](./covenant.md) could force sponsors to recursively send money to the same ACSA. So its anyone-can-spend, but only back to itself.

## Federated Sidechains

M-of-N multisig controls money, takes deposits and issues other tokens on another blockchain. No Soft Fork needed!

- Rootstock (RSK)
- Liquid by Blockstream

## Drivechains

???

Requires soft fork [BIP300](https://github.com/bitcoin/bips/blob/master/bip-0300.mediawiki)

## Rollups

Instead of putting a hash of the transaction set, submit a ZK Proof of the transaction set. Requires a Soft Fork

## Sources

[Bitcoin Magazine Article](https://bitcoinmagazine.com/technical/how-bitcoin-spacechains-work)
[4 minute explainer](https://www.youtube.com/watch?v=Ti1vRQMg_qE)
https://supertestnet.github.io/spacechain-launcher/
[2023 TabConf slides](https://docs.google.com/presentation/d/1Rvs6rO_6_CXAy7grftZugYtQGqAeiKRaDeUZ1sojkRQ/edit#slide=id.g22c75720d29_1_0)
[How Spacechain Work](https://bitcoinmagazine.com/technical/how-bitcoin-spacechains-work)
[SOMA](https://github.com/nbd-wtf/soma)