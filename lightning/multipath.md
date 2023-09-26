# MultiPath Payments (MPP & AMP)




## Advantages

Makes more efficient use of liquidity in the network. This also means lower barriers to entry for smaller nodes to route other payments and collect fees. 

## Disadvantages

- Requires more complex routing algorithms
- Error handling is harder. It's hard to know which parts of the payment failed and which reached successfully but were just being held until the other parts were received. 

## LND's implementation

(may be out of date)

LND implements MPP using a "recursive halvening" algorithm. if a part fails, just split it into two parts and try again. 

It is important not to launch all the parts at once, because they will all look at the same graph and think they all have full access to the liquidity and they might compete for it and cause one another to fail. 


## Sources

[Lightning Labs Blog](https://lightning.engineering/posts/2020-05-07-mpp/)