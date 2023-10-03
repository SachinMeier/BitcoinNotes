# Anchor Outputs

An Anchor output is an extra output included on a Lightning commitment transaction in order to ensure that both peers in a channel can have some say in setting the fee by CPFPing the commitment transaction in the case of a force-close and pinning attack. 

## CPFP Carve Out

The Anti-DoS rules for Bitcoin allow only up to 24 descendants to CPFP a given unconfirmed transaction. HOWEVER, a single extra CPFP can also be included if it spends an output of the original parent transaction AND the output is less than 1vkB. 

Lightning commitment transactions include 2 anchor outputs on each commitment transaction to allow both peers to take advantage of this rule.



## Sources

https://fanismichalakis.fr/posts/anchor-outputs/

[OpTech on CPFP Carve Out](https://bitcoinops.org/en/topics/cpfp-carve-out/)