# Silent Payments

Single re-usable address, privacy of a new address each time. (no interaction req'd)

Sender does a Diffie Hellman with the static address to generate a new ephemeral address.

Receiver must scan all UTXOs to find the payment. They DH with the static address to find the ephemeral address.

## Sources

[BIP351](https://github.com/bitcoin/bips/blob/master/bip-0351.mediawiki)