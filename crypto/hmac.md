# Hashed Message Authentication Code (HMAC)

## MAC

Message Authentication Code (MAC) is a short piece of information used to authenticate a message. It is used to verify both the integrity and authenticity of a message.

A MAC of a message and secret key cannot be forged without knowing the secret key. A MAC of the same message produced by a differnt key looks unrelated, and replay attacks do not work (seeing a MAC of a message does not allow you to produce a MAC of a different message).


## HMAC

HMAC is a MAC that uses a cryptographic hash function. It is used to verify the integrity and authenticity of a message. Basically, you concatenate the key and the message and hash them. This obviously relies on the cryptographic hash function being secure (being a one way function). 

This simple explanation is incomplete because it turns out to be insecure even if the hash function is secure. This is due to length extension attacks. 

## Length Extension Attacks

An attacker can use `H(m1)` and `|m1|` (length of `m1`) to calculate `H(m1 || m2)` for an arbitrary m2. This is because the hash function is deterministic and the length of `m1` is known.

MD5, SHA-1, and SHA-2 are all vulnerable to length extension attacks.


## HMAC v MAC

HMAC is a type of MAC that uses a cryptographic hash function.

```
HMAC construction actually provides (as long as the underlying hash function satisfies the appropriate assumptions) stronger security properties than what's required of a MAC. For example, nothing in the definition of a secure MAC algorithm (resistance to existential forgery under a chosen-plaintext attack) says that the MAC output can't reveal information about the plaintext to an attacker...HMAC, however, is guaranteed not to reveal any information about the plaintext as long as the underlying hash function is secure.
```

[- HMAC vs MAC](https://crypto.stackexchange.com/questions/2936/hmac-vs-mac-functions)

MACs which preserve the secrecy of the message are called `Privacy Preserving MAC`s (PP-MACs). HMAC is a PP-MAC.

HMACs, unlike some other MACs, do not require secure Initialization Vectors (IVs).

## HMAC Construction

from copilot:

HMAC is defined as:
    
    HMAC(K, m) = H((K' ^ opad) || H((K' ^ ipad) || m))

where:

- `K` is the secret key
- `m` is the message
- `H` is a cryptographic hash function
- `K'` is the secret key padded to the block size of `H`
- `ipad` is the byte `0x36` repeated `blocksize` times
- `opad` is the byte `0x5c` repeated `blocksize` times


## Sources

Copilot
[StackExchange on HMAC](https://security.stackexchange.com/questions/20129/how-and-when-do-i-use-hmac/20301#20301?newreg=d0844bd2683c46459f018805b2142fc1)
[Wiki: Length Extension Attack](https://en.wikipedia.org/wiki/Length_extension_attack)