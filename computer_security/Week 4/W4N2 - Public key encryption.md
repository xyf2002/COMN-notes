A public-key encryption system consists of:
- A key generation algorithm $G:\rightarrow K\times K$
- An encryption algorithm $E:K\times M\rightarrow C$
- A decryption algorithm $D:K\times C\rightarrow M$
such that $\forall(sk,pk)\in G$, and $\forall m\in M, D(sk,E(pk,m))=m$
($sk$ is the secret key, which is used for decryption; $pk$ is the public key, which is used for encryption)

The secret key $sk_{Bob}$ is known only to Bob, the public key $pk_{Bob}$ is known to everyone. $sk_{Bob}\ne pk_{Bob}$

# Mathematical backing
$a$ and $b$ in $\mathbb{Z}$ are **relative primes** if they have no common factors

The **Euler function** $\phi(n)$ is the number of elements that are relative primes with $n$:
$$
\phi(n)=|\{m|0<m<n\text{ and }\gcd(m,n)=1\}
$$

If $p$ is prime then $\phi(p)=p-1$
If $p$ and $q$ are prime, then $\phi(p\cdot q)=(p-1)(q-1)$

Let $n\in\mathbb{N}$. We define $\mathbb{Z}_n=\{0,...,n-1\}$
$$
\forall a\in\mathbb{Z},\forall b\in\mathbb{Z}_n,a\equiv b\mod{n}\Leftrightarrow\exists k\in\mathbb{N}.a=b+k\cdot n
$$

Modular inversion: the inverse of $x\in\mathbb{Z}_n$ is $y\in\mathbb{Z}_n$ such that $x\cdot y\equiv 1\mod n$. We denote $x^{-1}$ the inverse of $x\mod n$.
E.g. $7^{-1}$ in $\mathbb{Z}_{12}$: $7$
$4^{-1}$ in $\mathbb{Z}_{12}$: $4$ has no inverse in $\mathbb{Z}_{12}$

**Theorem**
Let $n\in\mathbb{N}$. Let $x\in\mathbb{Z}_n$. $x$ has an inverse in $\mathbb{Z}_n$ iff $\gcd(x,n)=1$ 

Let $n\in\mathbb{N}$. We define $\mathbb{Z}^*_n=\{x\in\mathbb{Z}_n|\gcd(x,n)=1\}$ i.e. $\mathbb{Z}^*_n$ contains all values in $\mathbb{Z}_n$ which have an inverse.
E.g. $\mathbb{Z}^*_{12}=\{1,5,7,11\}$

Note that $|\mathbb{Z}^*_n|=\phi(n)$

**Euler theorem**
$\forall n\in\mathbb{N},\forall x\in\mathbb{Z}^*_n,\text{ if }\gcd(x,n)=1\text{ then }x^{\phi(n)}\equiv1\mod n$
and, $\forall p$ prime, $\mathbb{Z}^*_n$ is a cyclic group, i.e.
$$
\exists g\in\mathbb{Z}^*_p,\{1,g,g^2,...,g^{p-2}\}=\mathbb{Z}^*_p
$$

# Intractable problems
- **Factoring**
	- Input: $n\in\mathbb{N}$
	- Output: $p_1,...,p_m$ primes such that $n=\prod^m_{i=0}p_i$
- **RSAP**
	- Input: $n$ such that $n=p\cdot q$ with $2\leq p,q$ primes
			  $e$ such that $\gcd(e,\phi(n))=1$
			  $m^e\mod n$
	- Output: $m$
- **Discrete log**
	- Input: prime $p$, generator $g$ of $\mathbb{Z}^*_p$,$y\in\mathbb{Z}^*_p$
	- Output: $x$ such that $y=g^x\mod p$
- **DHP**
	- Input: prime $p$, generator $g$ of $\mathbb{Z}^*_p,g^a\mod p,g^b\mod p$
	- Output: $g^{ab}\mod p$

# Diffie-Hellman (DH) protocol
We fix a very large prime $p$, and $g$ generator of $\mathbb{Z}^*_p$.
![[w4n2DhProtocol.png]]
## MitM attack on DH
![[w4n2DhMitmAttack.png]]
Here, the attacker is able to eavesdrop on messages between Alice and Bob, and can change or block messages as both Alice and Bob assume that the values they received from the attacker actually came from their counterpart.