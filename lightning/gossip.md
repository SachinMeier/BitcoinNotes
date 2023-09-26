# Gossip Messages

## Channel Announcements

- Prove ownership of funding txid
- Prove node owns persistent ID (node_id)
- Nodes agree on announcement message 
- only announce once per node in channel

## Node Announcements

- Node adds info to pubkey
- Node announcements are ignored if we don't know about one of their channels
- Indicate willingness to accept IPv4, IPv6, TOR, etc
- strictly increasing timestamp

## Channel Updates

- Update Channel Policy
- Set per-node in a channel
- Used to keep-alice after 2 weeks
- ~40% of gossip traffic is channel updates

## Problem: Unknown Channel Balance

Channel Capacity is gossiped, not balance. Capacity is not helpful for routing.

Solutions: 
- Iteraftive Routing
    - Retry excluding the failing edge
    - Tradeoff: Dijkstra's algorithm is O(n^2) (expensive)
- JIT Rebalancing
    - Routing nodes rebalance 
    - Expensive for routing node and requires multiple channels
    - rebalance can fail

## Problem: Out of Date Channel Updates
- Insufficient Fee / Timelock (CLTV Delta) causes Failure
- 10-15 minutes for gossip to propagate
- Restart a node and get flooded with update gossip

Solution:
- Re-Query for Update
    - Re-query for channel update if it's out of date
    - Tradeoff: Re-query is expensive
- Gossip Improvement
    - Inventory based gossip (unlike flooding). IDK learn more


## Problem: Light Client Routing
- Memory of Graph
    - Solution:
        - Prune the Graph
        - Learn expected neighborhoods
        - Tradeoffs: higher failure

- Bandwidth of Gossip
    - Solution:
        - Gossip Compression
        - Gossip Filtering
        - Gossip Relay

- Compute of routes


## Offload computation?

Let someone else do the computation/graph maintenance

[Trampoline Routing](./trampoline.md)
