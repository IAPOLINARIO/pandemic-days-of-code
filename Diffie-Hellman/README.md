# Implement Diffie-Hellman

For one of the most important algorithms in cryptography.
DIFFIE HELLMAN ALGORITHM (DH) Diffie Hellman (DH) key exchange algorithm is a method for securely exchanging cryptographic keys over a public communications channel. Keys are not actually exchanged – they are jointly derived. It is named after their inventors Whitfield Diffie and Martin Hellman.

If Alice and Bob wish to communicate with each other, they first agree between them a large prime number p, and a generator (or base) g (where 0 < g < p).

Alice chooses a secret integer a (her private key) and then calculates g^a mod p (which is her public key). Bob chooses his private key b, and calculates his public key in the same way.

Alice and Bob then send each other their public keys. Alice now knows a and Bob’s public key g^b mod p. She is not able to calculate the value b from Bob’s public key as this is a hard mathematical problem (known as the discrete logarithm problem). She can however calculate (g^b)^a mod p = g^ab mod p.

Bob knows b and g^a, so he can calculate (g^a)^b mod p = g^ab mod p. Therefore both Alice and Bob know a shared secret g^ab mod p. An eavesdropper Eve who was listening in on the communication knows p, g, Alice’s public key (g^a mod p) and Bob’s public key (g^b mod p). She is unable to calculate the shared secret from these values.

In static-static mode, both Alice and Bob retain their private/public keys over multiple communications. Therefore the resulting shared secret will be the same every time. In ephemeral-static mode one party will generate a new private/public key every time, thus a new shared secret will be generated.

### Example

Set a variable "p" to 37 and "g" to 5.

Generate "a", a random number mod 37. Now generate "A", which is "g" raised to the "a" power mode 37 --- A = (g\*\*a) % p.

Do the same for "b" and "B".

"A" and "B" are public keys. Generate a session key with them; set "s" to "B" raised to the "a" power mod 37 --- s = (B\*\*a) % p.

Do the same with A\*\*b, check that you come up with the same "s".

To turn "s" into a key, you can just hash it to create 128 bits of key material (or SHA256 it to create a key for encrypting and a key for a MAC).

Ok, that was fun, now repeat the exercise with bignums like in the real world. Here are parameters NIST likes:

```
p:
ffffffffffffffffc90fdaa22168c234c4c6628b80dc1cd129024
e088a67cc74020bbea63b139b22514a08798e3404ddef9519b3cd
3a431b302b0a6df25f14374fe1356d6d51c245e485b576625e7ec
6f44c42e9a637ed6b0bff5cb6f406b7edee386bfb5a899fa5ae9f
24117c4b1fe649286651ece45b3dc2007cb8a163bf0598da48361
c55d39a69163fa8fd24cf5f83655d23dca3ad961c62f356208552
bb9ed529077096966d670c354e4abc9804f1746c08ca237327fff
fffffffffffff

g: 2

```

Note that you'll need to write your own modexp (this is blackboard math, don't freak out), because you'll blow out your bignum library raising "a" to the 1024-bit-numberth power.

You can find modexp routines on Rosetta Code for most languages.
