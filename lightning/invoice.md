# Lightning Invoices

## Payment Secret

A payment secret is a secret included in the invoice to allow the receiver to prevent hops in the route from probing to discover the end recipient identity once they see the payment hash. The sender includes the secret in the last layer of the onion, so that only the receiver sees it and can check that the sender was the one who saw and paid the invoice. 

Note: LND calls this the payment address