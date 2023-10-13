A **symmetric cipher** consists of two algorithms:
- An encryption algorithm $E:K\times M\rightarrow C$
- A decryption algorithm $D:K\times C\rightarrow M$
$\forall k\in K,\forall m\in M, D(k,E(k,m)) = m$

This means that the encryption algorithm takes a key and a message, and produces an encrypted message, and the decryption algorithm takes the same key and the encrypted message, and returns the original message.

# Uses
- File encryption
	- The data gets en/decrypted using the same key, such as from a password. In this case it will likely be the same user encrypting and decrypting the file
- Secure communication:
	- In this case, the key for symmetric communication must be shared between the two parties (generally by starting with an asymmetric encryption scheme)

# What makes a good encryption scheme
An encryption scheme is secure against a given adversary, if this adversary cannot:
- recover the secret key $k$ from a cipher text
- recover the plaintext $m$ underlying a ciphertext $c$
- recover any bits of the plaintext $m$ underlying a ciphertext $c$

# Kerckhoff's principle
The only part of a cryptographic system that should be hidden are the secret keys. The architecture, design, and algorithm should be public, as this allows many users, white hat hackers, academics, etc. to scrutinise the system, enabling early discovery and corrections of flaws or vulnerabilities.

# Adversary capabilities
A cryptographic scheme is secure under some assumptions (against a certain type of attacker)
A cryptographic scheme may be vulnerable to certain types of attacks but not others

The attacker knows the encryption/decryption algorithm, but may also have access to:
- **ciphertext only attack** - some cyphertexts $c_1,...,c_n$
- **known plaintext attack** - some plaintext/cyphertext pairs $(m_1,c_1),...,(m_n,c_n)$ such that $c_i=E(k,m_i)$
- **chosen plaintext attack** - the attacker has access to an encryption oracle - can perhaps trick a user into encrypting messages $m_1,...,m_n$ of his choice
- **chosen ciphertext attack** - he has access to a decryption oracle - can perhaps trick a user into decrypting ciphertexts $c_1,...,c_n$ of his choice
- either unlimited, polynomial, or realistic ($\leq 2^{80}$) **computational power**

# Brute force attack
All encryption schemes are vulnerable to brute force attacks - i.e. by trying all possible keys we can find which key works to decrypt a message. This does still require some knowledge of what the ciphertext should decrypt to, in order to know when the correct message is found.

This is fairly simple to prevent by:
- Ensuring $K$ is sufficiently large, i.e keys should be sufficiently long
	- If we know all keys are between 0-100 then it is trivial to check all of them
	- $K$ tends to have 128+ bits
- Keys should be sampled uniformly at random from $K$
	- If we have a 128 bit key, but keys are assigned sequentially from 0, then it is likely easy to find they key used in any case, as it is unlikely to give out keys in scales greater than billions

# Perfect secrecy
A cipher $(E,D)$ over $(M,C,K)$ satisfies perfect secrecy if for all messages $m_1,m_2\in M$ of the same length ($|m_1|=|m_2|$), and for all ciphertexts $c\in C$
$$
|Pr(E(k,m_1)=c)-Pr(E(k,m_2)=c)|\leq\epsilon
$$
where $k\leftarrow K$  and $\epsilon$ is some "negligible quantity".

Essentially, no matter what ciphertext you have, you cannot say anything about what the key or message were. This does not however guarantee that you can't get information about the keys/messages if you have many ciphertexts.

# One-Time Pad (OTP)
In an OTP, the message, key, and ciphertext are all bitstrings with the same length, i.e $M=C=K=\{0,1\}^n$. 
The encryption algorithm is to simply XOR the key with the message ($\forall k\in K. \forall m\in M.E(k,m)=k\oplus m$).
Decryption just requires XORing the key with the ciphertext ($\forall k\in K. \forall c\in C.D(k,c)=k\oplus c$).

An OTP satisfies [[W3N2 - Symmetric encryption#Perfect secrecy|perfect secrecy.]]

## Two-time pad attacks
If the same OTP key is used for both $m_1$ and $m_2$, then by XORing the ciphertexts together you get $m_1\oplus m_2$
![[w3n2TwoTimePadAttack.png]]
Generally, you can then obtain both original messages from this as there are patterns in languages and images.

## Limitations of OTPs
- Key length
	- The key must be at least as long as the plaintext
- Key sharing
	- It is difficult to share the keys for a two way conversation. Generally, both parties must share the keys in some insecure way, agree on the order in which they will be used, and then the conversation can occur. This can happen, but generally isn't done over a network. e.g. giving someone a physical disk/book of keys.
- Requires true randomness
	- If the key is not truly random then frequency analysis may be possible.
- Perfect secrecy does not guarantee security against all possible attacks
- Malleability
	- If we know, or strongly suspect, that a specific part of the message is a specific string, e.g. a header, then by XORing that string with the message we can recover the key for that part of the message. Therefore it is possible to modify known sections of the ciphertext without being able to decrypt the entire ciphertext.
# Stream ciphers
Stream ciphers aim to make OPT practical.

This uses a pseudorandom generator (PRG) to generate a key from a key seed:
$G:\{0,1\}^s\rightarrow\{0,1\}^n$ with $s << n$.

Using a PRG $G$:
Encryption: $E(k,m)=G(k)\oplus m$
Decryption: $D(k,c)=G(k)\oplus c$

Stream ciphers:
- Are much easier to manage key sharing, as keys are much shorter than messages. E.g you could include in each message the key seed used in the next message
- Are still vulnerable to two-time pad attacks

# Block ciphers
A block cipher encrypts a bitstring by dividing it into fixed length blocks and encrypting each block using the key.

It consists of:
- an encryption algorithm $E:\{0,1\}^k\times\{0,1\}^l\rightarrow\{0,1\}^l$
- a decryption algorithm $D:\{0,1\}^k\times\{0,1\}^l\rightarrow\{0,1\}^l$
where $k$ and $l$ are the lengths of the key and block respectively.

Current examples include:
- 3DES: $l=64,k=168$
- AES: $l=128,k=128,192,256$

# Triple DES (3DES)
3DES uses repeated application of DES encryption.
It is used in bank cards and RFID chips.

Let $DES=(E_{DES},D_{DES})$ and $3DES=(E_{3DES},D_{3DES})$.
$E_{3DES}:(\{0,1\}^k)^3\times\{0,1\}^l\rightarrow\{0,1\}^l$
$E_{3DES}((K_1,K_2,K_3),M)=E_{DES}(K_1,D_{DES}(K_2,E_{DES}(K_3,M)))$

(This encrypt then decrypt then encrypt again is for backwards compatibility. A 3DES implementation will also be able to encrypt and decrypt DES ciphers by simply setting $K_1=K_2=K_3$.)

$D_{3DES}:(\{0,1\}^k)^3\times\{0,1\}^l\rightarrow\{0,1\}^l$
$E_{3DES}((K_1,K_2,K_3),C)=D_{DES}(K_3,E_{DES}(K_2,D_{DES}(K_1,C)))$
The cost of 3DES is that it takes 3 times the runtime as DES.
3DES beats DES as it has a larger key size$=3\times56=168$ bits, which results in an exhaustive search space of $2^{168}$.

There is however a simple meet-in-the-middle attack in time $2^{118}$.

## 2DES and meet in the middle attacks
$E_{2DES}((K_1,K_2),M)=E_{DES}(K_1,E_{DES}(K_2,M))$
This has an exhaustive search time complexity of $2^{112}$, however, this is much weaker than 3DES, as we know that, for $m$ and $c$ such that $E_{2DES}((k_1,k_2),m)=c$ we have $E_{DES}(k_2,m) = D_{DES}(k_1,c)$.

Therefore, we can compute all possible $E_{DES}(k_2,M)$ and sort the results, then compute all possible $D_{DES}(k_1,C)$ and sort those results, then find the pair of keys which result in a matching value. Each of these steps is of time complexity $2^{56}\log(2^{56})$, or approximately $2^{63}$.