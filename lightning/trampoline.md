# Trampoline Payments

Trampoline payments are a proposed way to route payments through the network which mitigate the burdens of source-based routing. 

## Source Based Routing 

Source based routing is currently used LN. The sender of a payment must maintain a sufficient view of the network and build the full route to their end destination. This cannot scale very well as the graph grows faster than compute power of small devices. 

## Trampoline Payments

Trampoline payments allow some nodes to build partial paths and other less powerful nodes to blindly use those partial paths to send payments. So if Alice wants to send to Charlie, but doesn't know how, she can send a trampoline payment to Bob, who knows how to get to Charlie. 

Trampoline onions are slightly different from regular onions. But are still exclusively created by the payer, so sacrifices no privacy. 

Trampoline route (created by Alice):

```
Alice ---> Bob Pubkey ---> Carol Pubkey ---> Dave Pubkey
```

Complete route 
- A is calculated by Alice
- B is calculated by Bob
- C is calculated by Carol

```
        +--> (A) --+          +--> (B) --+            +--> (C) --+
        |          |          |          |            |          |
        |          |          |          |            |          |
Alice --+          +--> Bob --+          +--> Carol --+          +--> Dave

<----------------------------------><----------------------><--------------------->
            (normal payment)            (normal payment)        (normal payment)
```
- [source](https://github.com/lightning/bolts/blob/trampoline-routing-no-gossip/proposals/trampoline.md)

So the Trampoline Onion will be placed inside the last `hop_payload` of the normal onion, which would normally not have anything. Alice would construct the regular onion to get the payment to Bob, but then include a Trampoline Onion instructing Bob to send the payment to Carol, but only by Pubkey, since she doesn't know the graph well enough to build a route. In order for Bob to send to Carol, he must know the route to Carol. So Bob will build a regular onion to send to Carol, but include a Trampoline Onion instructing Carol to send the payment to Dave.

## Privacy

Privacy is great. Since normal onions are used between each Trampoline, the Trampoline Onion is only visible to the Trampoline nodes, and the same privacy guarantees apply to trampolines: They don't know the sender and can't tell if the next hop is the destination or another trampoline. 

If some people are using trampoline payments, and some are not, then the trampoline nodes will be able to guess if the next trampoline destination is the recipient or not. But this is always the case without trampoline payments. 


## Fee Estimation 

Because you run a node who doesn't know the graph, you'll have to overestimate on fees and trampoline nodes can claim the excess. 


## Rendezvous Routing

Rendezvous routing is a proposed way to route payments through the network which mitigate the burdens of source-based routing and the privacy downsides of receiving. 


Middleman advertises rendezvous services. Other nodes can use the middleman to send payments to a destination without knowing the destination. Both sender and receiver build routes between themselves and the middleman. The middleman then forwards the concatenates the routes.