# Why was Segwit important for Lightning

Segwit eliminated transaction malleability, which was a problem for Lightning. A malleable transaction could never be safe to use for a Lightning Channel because the commitment transaction could be changed by a malicious party, which would invalidate the HTLCs.