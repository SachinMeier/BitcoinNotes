# Lightning Fees

Lightning fees are charged on outbound forwarding. Each channel defines a base-fee and a per-volume fee rate, measured in parts-per-million (sats per million sats) or ppm. 


The smallest possible payments which have a real HTLC associated with them are related to the base fees and ppm fee of the hops. Each channel policy also defines a minimum HTLC value, which is the smallest HTLC that can be forwarded. 

If a channels is force closed with an open HTLC, the HTLC will be dropped if its below the dust threshold. So, HTLCs below the dust threshold are more trust-based since they cannot be settled onchain. 