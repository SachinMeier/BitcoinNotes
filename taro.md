# Taro (Taproot Assets)


## Sparse Merkle Trees

A Sparse Merkle Tree is a Merkle Tree with 2^256 leaves. Each leaf is thus a 32-byte ID that is also the binary path from the root down to the leaf. 

Because most of the tree is empty, it can be easily compressed and computed. Because the 32-byte ID indicates the only possible location of the item in the tree, we can efficiently create Exclusion Proofs, which prove that an item is *not* in tree.

## Merkle Sum Tree

A Merkle Sum Tree is a Merkle Tree where each node contains the sum of its children. This allows us to efficiently compute the sum of any subset of the tree.

## MS-SMT 

Taro uses Merkle Sum Sparse Merkle Trees (MS-SMTs) to store the balances of each asset. 



## Asset Splits



### Internal Transfers

Central coordinator controls the Asset merkle tree. But individual owners control the keys for asset leaves. 

Alice can sign off on a transfer of her asset to Bob. She sends this proof to the coordinator, who updates the tree. 

Receiving this way is interactive.

## vPSBTs

vPSBTs are a new type of PSBT that allow for communication about a new state of the MS-SMT, while an anchor PSBT (actual PSBT) is the underlying PSBT for the bitcoin transaction. 

vPSBTs have many more fields that are specific to Taro Asset transfers.

## Universe

Who hosts the MS-SMT since it never actually hits the blockchain? A Universe. Like a block explorer, but for Taro Assets. It would make sense for Asset Issuers/Controllers to host their own Universes or at least host all the proofs for their assets.

## Taro on Lightning

Taro Assets can be sent over [Lightning](./lightning/lightning.md).

Alice has a taro channel with bob. Bob and Charlie have a bitcoin channel. Charlie and Dave have a taro channel. Alice can send taro to Dave through Bob and Charlie. Bob will convert the taro asset to bitcoin and send it to Charlie. Charlie will convert the bitcoin to taro and send it to Dave. Bob and Charlie will each take a fee for their service and have their own conversion rates. 

Alice <--TARO--> Bob <--BTC--> Charlie <--TARO--> Dave