# Lightning's Layers

Multi-Hop: Sphinx
Transfer: HTLC
Update: LN-Penalty, DMC, Eltoo, Gossip
Base: Framing & Feature Negotiation
Transport: Noise_XK

## Multi-Hop: Sphinx

Sphinx is a packet format that allows for onion routing. It is used to route payments through the network.

Goals:
- Route payments through network
- Maintain privacy

Sphinx Onion Routing

- Source based routing (sender determines entire route)
- Onion encryption: Sender creates unique DH encryption keypair with EACH member of the route. Each member of the route can only decrypt the outer layer of the onion. Any node on the route only knows who sent it to them and who they send it to, not source or destination.
- Sphinx Construction.
    - LN uses a 20-hop max. 
    - Perfect privacy for sender. Ok privacy for receiver.


Onion Construction:  (no payment hash included?)
- Header:
    - Version Byte
    - sender's 33-byte public key (NOT node pubkey for privacy)
       this payment-specific pubkey is used by each hop to derive a shared secret with the sender (DH) using the recipient's node pubkey (static)
- Payload:
    - 1300 byte hops_data
        - 20 * 65 bytes 
            - Realm: 1 byte 
            - per_hop: 32 bytes
                - Short channel ID: 8
                - amt_to_forward: 8
                - outgoing_cltv_value: 4
                - padding: 12
            - hmac: 32 bytes
- HMAC

[Onion route](https://docs.google.com/presentation/d/1-eyceLlSmcLpbPJLzj6_CnVYQdo1AUP3y5XD716U-Lg/edit#slide=id.g5c52c589eb_4_1407)

Onions are the payload for the `update_add_htlc` message.

Onions are always 1366 bytes (to avoid giving away hints at where a router is in the route)

## Transfer: HTLC

Goals:
- Move value between parties
- Atomic, trustless payments

Hashed Time-Locked Contract (HTLC)

"If you reveal this secret before this timelock, you can take the funds. Otherwise, I can take the funds back."

Update layer leaks into HTLCs as well because they also need to be revocable. 

    +-------+                               +-------+
    |       |--(1)---- update_add_htlc ---->|       |
    |       |--(2)---- update_add_htlc ---->|       |
    |       |<-(3)---- update_add_htlc -----|       |
    |       |                               |       |
    |       |--(4)--- commitment_signed --->|       |
    |   A   |<-(5)---- revoke_and_ack ------|   B   |
    |       |                               |       |
    |       |<-(6)--- commitment_signed ----|       |
    |       |--(7)---- revoke_and_ack ----->|       |
    |       |                               |       |
    |       |--(8)--- commitment_signed --->|       |
    |       |<-(9)---- revoke_and_ack ------|       |
    +-------+                               +-------+

1. Alice wants to send to Bob
2. Alice sends `update_add_htlc` to Bob with a payment hash, amount, and a timelock using the onion payload. 
3. Alice sends `commitment_signed` to Bob, sending a signed Commitment Transaction with the HTLC output. At this point, Bob can actually broadcast the old (without HTLC) AND the new state (with HTLC)
4. Bob revokes older commit tx via `revoke_and_ack` msg. Now he can only broadcast the new state (with HTLC). But Alice can still broadcast the old state, but this is not a problem because Alice wants the payment to go through
5. Bob sends `commitment_signed` to Alice, sending a sig for Alice's Commitment Transaction with the HTLC output.
6. Alice still able to publish both txs, but she wants the payment to go through, so she sends `revoke_and_ack` to Bob, revoking her old state. 
...Bob can now route the payment to the next hop.

`update_fulfill_htlc` returns the preimage. 

## Update: LN-Penalty, DMC, Eltoo, Gossip

Goals:
- Create & Update state of channel 
- Trustlessly invalidate old states

### Lightning Network Penalty (LN-Penalty)

If peer broadcasts an old state, you can use the LN-Penalty to take all their funds.

Revocation transactions:
- Each party give the other a key that can be used to take all of their funds if they broadcast an old state.
- This revocation key is double-blinded. Each party has half of the key when a new commitment transaction is created. To revoke said transaction, each party must reveal their half of the key to the other. 

The reason you need both peers to have different, asymetric revocable commitment transactions with different revocation keys is to prevent a malicious peer from broadcasting an old state and then using the revocation key to take all of the other peer's funds. Each peer has a commitment transaction which does NOT have revocation on outputs to their peer. Only outputs to_local have revocation paths. 

See: [HTLCs](./htlc.md)

Closing Channels:

- Collaborative Close
    Sign a new transaction with no timelocks and broadcast it. 
- Force Close
    Broadcast the latest commitment transaction. Timelocks are present. 
- Breach Close
    Broadcast an old commitment transaction. Other party should revoke. 

Cons of LN Penalty: 
- Maintaining state is critical. If you lose state, you lose funds.

Solutions: Watchtower. 

### Past update mechanisms: 

Simple payment channels:

1 party opens a channel to a 2-of-2 with another, with all funds on their side. They sign a transaction and hand it over every time they want to pay the peer. The peer can broadcast the transaction at any time, but has the incentive to keep the channel open since they can only receive more funds. 

Duplex Micropayment Channels (DMC):
- Allow bi-directional channels
- revoke old states by starting with a very high timelock and then decreasing it over time. 
- Limited by absolute time. 

## Base: Framing & Feature Negotiation

Goals:
- How to interpret messages (serialization standards)
- Signal feature support 

Message Framing: 

- Type: 2 bytes
- Length: varint

"OK to be odd" rule: unrecognized odd-numbered types are allowed (ignored), but even-numbered types are not. Unrecognized even-numbered types must be rejected and the connection severed

TLV (Type-Length-Value) fields

Messages:

- INIT
    - Type 16
    - Local Length
    - Local features

## Transport: Noise_XK

Goals:
- Authenticate Node Identity
- Fingerprint resistant
    - You cannot identify a node just by connecting
- Transport Encryption

Noise_XK uses the unknown/known Variation. It is Encrypted, Authenticated communication protocol. 

Node announces listening addr & sign. You connect to known node, but do not announce your outbound IP. The initiator of the connection is the unknown party. The responder is the known party.

Lightning uses persistent identities (node pubkeys)

Nodes perform Diffie-Hellman key exchange to establish a shared secret. This is used to derive a set of keys for encryption and authentication.

## Sources

https://btctranscripts.com/scalingbitcoin/tel-aviv-2019/edgedevplusplus/lightning-network-layer-by-layer/
Youtube: https://www.youtube.com/watch?v=tie0Gpq2eJI
[Multihop Layer](https://btctranscripts.com/chaincode-labs/chaincode-residency/2019-06-24-rene-pickhardt-multihop-in-lightning/)
