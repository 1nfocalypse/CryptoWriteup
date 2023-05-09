<p align="center">
  <a href="https://github.com/1nfocalypse/CryptoWriteup">
	<img alt="1nfocalypse - Mathematical Cryptography" src="https://i.imgur.com/gvXGf7x.png">
  </a>
</p>
<p align="center">
  <a href="https://choosealicense.com/licenses/cc-by-sa-4.0/">
  	<img alt="License: CC-BY-SA-4.0" src="https://img.shields.io/github/license/1nfocalypse/CryptoWriteup">
  </a>
</p>
<h2 align="center">Cryptography</h3>
<h3 align="center">
  A Writeup on Mathematical Cryptography, from Shift Ciphers to Elliptic Curve Groups
</h2>
<p align="center">
  By <a href="https://github.com/1nfocalypse">1nfocalypse</a>
</p>

## Contents

- [Summary](#summary)
- [Some Background](#some-background)
- [Terms](#terms)
- [Shift Ciphers](#shift-ciphers)
- [Vigenère Cipher](#vigenère-cipher)
- [Feistel Network](#feistel-networks)
- [More Background](#more-background)
- [RSA](#rsa)
- [ElGamal](#elgamal)
- [Elliptic Curves](#elliptic-urves)
- [Additional Information](#additional)
- [Recommended Reading](#recommended)
- [Practice Problems](#practice-problems)
- [Solutions to Selected Problems](#solutions-to-selected-problems)


## Summary

$\mathbf{Cryptography}$ is the field of study regarding communication in an adversarial, or hostile, environment. It does this via obfuscating the original information or message via a mathematical function, dubbed encryption, such that only the intended recipient is able to decode the message, rather than any eavesdropper. As such, it is the job of the sender and the intended to recipient to practice cryptography, and the job of the eavesdropper to practice $\mathbf{cryptanalysis}$. Cryptanalysis is the study of how to break these functions such that the eavesdropper is able to read the original message without having the appropriate means of decrypting the communication. These fields together form the overall field called $\mathbf{Cryptology}$.

This document is intended to serve as a reference guide along with as a resource for mathematical cryptography. While it may be useful to utilize it to supplement for self study, it is not intended to replace an appropriate curriculum. This document is also not meant to be a comprehensive guide to cryptography, and as such, not all systems or features will be talked about, i.e. NTRU, signatures, etc. There will, however, be recommended reading delineated at the bottom of this guide that will be much more comprehensive. This guide assumes knowledge of algebra and some number theory, however, anything past high school level will be explained, albeit with some brevity. It is highly encouraged to pursue study in these areas yourself for further information. 


Most informationation is sourced from [Coding Theory and Cryptography: The Essentials 2Ed](https://www.amazon.com/Coding-Theory-Cryptography-Essentials-Mathematics/dp/0824704657/), Wikipedia, and personal notes from a university mathematics course. Some of the contents here either have or will have toy implementations of them in C++ on my main profile. These projects are ongoing, and as such may not be completed or fully functional by the time of reading. Current implementations are:
- [Feistel Network](https://github.com/1nfocalypse/Feisty)


## Some Background
Throughout, mathematical notation and terminology will be utilized. This section serves to familiarize people who have never encountered this before. If you are comfortable with mathematics, please feel free to skip this section.

$\mathbf{set}$ : A set is a collection of elements, in this case, numbers. Some sets here will be represented with uppercase letters, whereas the elements within them will be referred to with lowercase letters when relevant. They are declared in a manner such as X = {a,b,c,...} such that a,b,c... are elements of X, with different modifiers as required.

$\in$ : This symbol denotes that a particular value is in a set. For example, x $\in$ X denotes that x is an element within the set X.

$\forall$ : This symbol, read "For all", denotes that every element x in a set follows some rule. For example, $\forall$ x $\in$ X, x = 1 is read for all x in the set X, x is equal to 1. This symbol is also called the Universal Quantifier

$\mathbb{N}$ : This is the symbol for the set of Natural Numbers, or numbers 0 through infinity. 

$\mathbb{Z}$ : This is the symbol for the set of Integers, or whole numbers from negative infinity through positive infinity.

| : This is the symbol meaning that something is given, or to be assumed. It is read given.

: : This is the symbol for "such that," and read in the same manner.

$\bigoplus$ : This is the symbol for a logical XOR, or exclusive or. This denotes an operation such that:
- $0 \bigoplus 0 = 0$
- $0 \bigoplus 1 = 1$
- $1 \bigoplus 0 = 1$
- $1 \bigoplus 1 = 0$

for each element in the bitstring.

$\exists$: This means that some element of a set must exist, or exist with some property.

$\mathbf{mod}$ : This is the modulus operator, which can be understood as the remainder when dividing the left number by the right. It may also be represented as %.

## Terms

Let us begin by introducing some terms utilized in mathematical cryptography. We will begin with $\mathscr{A}$, or the $\mathit{alphabet}$ $\mathit{space}$. The alphabet space is defined as a finite set of symbols for communication, such as our alphabet, or the binary set K = {0,1}. 

From this, we will then define the $\mathit{message}$ $\mathit{space}$, $\mathscr{M}$. The message space is a string of symbols from $\mathscr{A}$ that is to be communicated. For example, should we wish to send a string of 8 characters with $\mathscr{A}$ = {0,1}, we would denote $\mathscr{M}$ as $\\{ 0,1 \\} ^8$. 

We must also define the encryption and decryption functions, and along with them, the $\mathit{encryption}$ $\mathit{space}$, $\mathscr{C}$, and the $\mathit{key}$ $\mathit{space}$ $\mathscr{K}$.

The $\mathit{encryption}$ $\mathit{function}$ $E_{k}$ is any function such that $E_{k} : \mathscr{M} \mapsto \mathscr{C}$, with $\mapsto$ denoting that every value in $\mathscr{M}$ must have a corresponding value in $\mathscr{C}$ that will be the result. 

The $\mathit{decryption}$ $\mathit{function}$ $D_{k}$ is any function such that $D_{k} : \mathscr{C} \mapsto \mathscr{M}$.

The encryption space can be thought of simply as another message space, however, it is after the encryption function has been applied to the original message. The key space can be thought of simply as where the keys utilized in the encryption and decryption functions are stored. 

$\mathit{Symmetric}\\ \mathit{Key}\\ \mathit{Cryptography}$ is where the same key, k, is used for both encryption and decryption.

$\mathit{Asymmetric}\\ \mathit{Key}\\ \mathit{Cryptography}$ is where published, public keys are used for encryption, and unpublished, private keys are used for decryption.

With this in mind, let us proceed to the first $\mathit{cipher}$, or encryption scheme, the Shift Cipher.

## Shift Ciphers

Shift Ciphers are one of, if not the simplest, kinds of ciphers. Originating with Julius Caesar, and hence sometimes called the $\mathit{Caesar}$ $\mathit{Cipher}$, it generally utilizes $\mathscr{A}$ = {A,B,C,...,Y,Z}. Each value in $\mathscr{A}$ is then mapped to a numeric value X = {0,1,2,...,24,25}. This mapping is also utilized with the key, of which $\mathscr{K}$ = $\mathbb{N}$. Upon deciding upon a key value such that $k \in \mathbb{N}$, the encryption function is $E_{k} = m_{i} + k\\ \mathbf{mod}\\ 26$. As you might expect, the decryption function is $D_{k} = c_{i} - k\\ \mathbf{mod}\\ 26$. This results in the values of each letter being shifted to the right by k % 26 to encrypt, and to the left by k % 26 to decrypt. Clasically, Caesar was known to choose k = 3, and as such, so shall we.

Given the mappings:
- $A \leftrightarrow 0$
- $B \leftrightarrow 1$
- $C \leftrightarrow 2$
- and so on,

Let the $\mathit{cleartext}\$, or original message M, be "ILIKECRYPTOGRAPHY". Let k = 3. Applying the encryption function, we obtain:
- $C_{1} = I + 3 \rightarrow C_{1} = 8 + 3\\ \mathbf{mod}\\ 26= 11$
- $C_{2} = L + 3 \rightarrow C_{2} = 11 + 3\\ \mathbf{mod}\\ 26 = 14$
- $C_{3} = I + 3 \rightarrow C_{3} = 8 + 3\\ \mathbf{mod}\\ 26 = 11$
- $C_{4} = K + 3 \rightarrow C_{4} = 10 + 3\\ \mathbf{mod}\\ 26 = 13$
- $C_{5} = E + 3 \rightarrow C_{5} = 4 + 3\\ \mathbf{mod}\\ 26 = 7$
- $C_{6} = C + 3 \rightarrow C_{6} = 2 + 3\\ \mathbf{mod}\\ 26 = 5$
- $C_{7} = R + 3 \rightarrow C_{7} = 17 + 3\\ \mathbf{mod}\\ 26 = 20$
- $C_{8} = Y + 3 \rightarrow C_{8} = 24 + 3\\ \mathbf{mod}\\ 26 = 1$
- $C_{9} = P + 3 \rightarrow C_{9} = 15 + 3\\ \mathbf{mod}\\ 26 = 18$
- $C_{10} = T + 3 \rightarrow C_{10} = 19 + 3\\ \mathbf{mod}\\ 26 = 22$
- $C_{11} = O + 3 \rightarrow C_{11} = 14 + 3\\ \mathbf{mod}\\ 26 = 17$
- $C_{12} = G + 3 \rightarrow C_{12} = 6 + 3\\ \mathbf{mod}\\ 26 = 9$
- $C_{13} = R + 3 \rightarrow C_{13} = 17 + 3\\ \mathbf{mod}\\ 26 = 20$
- $C_{14} = A + 3 \rightarrow C_{14} = 0 + 3\\ \mathbf{mod}\\ 26 = 3$
- $C_{15} = P + 3 \rightarrow C_{15} = 15 + 3\\ \mathbf{mod}\\ 26 = 18$
- $C_{16} = H + 3 \rightarrow C_{16} = 7 + 3\\ \mathbf{mod}\\ 26 = 10$
- $C_{17} = Y + 3 \rightarrow C_{17} = 24 + 3\\ \mathbf{mod}\\ 26 = 1$ 

From this, we map the numbers resulting from $E_{3}$ to the letters in the same manner as we did previously in order to optain the $\mathit{ciphertext}$, or the encrypted text, which is:

C = LOLNHFUBSWRJUDSKB

Applying the decryption function, $D_{3}$, is a similar process, subtracting three instead of adding it mod 26, briefly demonstrated here:
- $M_{1} = L - 3 \rightarrow M_{1} = 11 - 3\\ \mathbf{mod}\\ 26= 8$
- $M_{2} = O - 3 \rightarrow M_{2} = 14 - 3\\ \mathbf{mod}\\ 26 = 11$
- $M_{3} = L - 3 \rightarrow M_{3} = 11 - 3\\ \mathbf{mod}\\ 26 = 8$
- and so on,

Repeating the mapping from numbers to letters with the output of $D_{3}$, we obtain M again, M being:

M = ILIKECRYPTOGRAPHY

Thus concluding the encryption and decryption of a Shift Cipher.

### But Wait, Why Don't We Use This?

Given that the Shift Cipher takes place over the English alphabet and has a constant k, one must only perform at most 25 shifts to be able to decipher the code, making it computationally inexpensive to break. A frequency analysis can also be performed, which uses the fact that certain characters occur at certain frequencies in the English language. By measuring the frequency at which characters occur in the ciphertext, it is possible to take an educated guess as to which characters they represent, and break the code in that manner. It also requires that it be given that a key can be communicated without risk of interception, as otherwise, the entire system would be vulnerable.


## Vigenère Cipher

The Vigenère Cipher is similar to a shift cipher, however, utilizes a variable value of k based on a $\mathscr{K}$ composed of a string, in turn making it a $\mathit{polyalphabetic}$ cipher. The Shift Cipher, conversely, is considered a $\mathit{monoalphabetic}$ cipher. By utilizing the variable k value, the Vigenère Cipher makes itself significantly less vulnerable to a frequency analysis and significantly more computationally complex to crack. Utilizing the same mapping of {A,B,C,...,Y,Z} $\leftrightarrow$ {0,1,2,...,24,25}, we find $E_{k}$ to be defined as $E_{k} = m_{i} + k_{i}\\ \mathbf{mod}\ 26$, and $D_{k}$ to be defined similarly as $D_{k} = c_{i} - k_{i}\\ \mathbf{mod}\\ 26$. An example of a Vigenère Cipher can be found below.

Let k = "ILIKE" and m = "CRYPTOGRAPHY"
- $k_{1}$ = I = 8
- $k_{2}$ = L = 11
- $k_{3}$ = I = 8
- $k_{4}$ = K = 10
- $k_{5}$ = E = 4

The above step is purely done for the reader's reference below.

- $C_{1} = C + I \rightarrow C_{1} = 2 + 8\\ \mathbf{mod}\\ 26= 10$
- $C_{2} = R + L \rightarrow C_{2} = 17 + 11\\ \mathbf{mod}\\ 26 = 2$
- $C_{3} = Y + I \rightarrow C_{3} = 24 + 8\\ \mathbf{mod}\\ 26 = 6$
- $C_{4} = P + K \rightarrow C_{4} = 15 + 10\\ \mathbf{mod}\\ 26 = 25$
- $C_{5} = T + E \rightarrow C_{5} = 19 + 4\\ \mathbf{mod}\\ 26 = 23$
- $C_{6} = O + I \rightarrow C_{6} = 14 + 8\\ \mathbf{mod}\\ 26 = 22$
- and so on.

The result of $E_{k}$ is C = KCGZXWRZKTPJ.

To decode a Vigenère Cipher, we simply subtract the value of $k_{i}$ from the value of the ciphertext, following the above given equation $D_{k}$.

- $M_{1} = K - I \rightarrow M_{1} = 10 - 8\\ \mathbf{mod}\\ 26= 2$
- $M_{2} = C - L \rightarrow M_{2} = 2 - 11\\ \mathbf{mod}\\ 26 = 17$
- $M_{3} = G - I \rightarrow M_{3} = 6 - 8\\ \mathbf{mod}\\ 26 = 24$
- $M_{4} = Z - K \rightarrow M_{4} = 25 - 10\\ \mathbf{mod}\\ 26 = 15$
- $M_{5} = X - E \rightarrow M_{5} = 23 - 4\\ \mathbf{mod}\\ 26 = 19$
- $M_{6} = W - I \rightarrow M_{6} = 22 - 8\\ \mathbf{mod}\\ 26 = 14$
- and so on.

The result of $D_{k}$ is M = CRYPTOGRAPHY, our original cleartext.

### Why Don't We Use It?

In 1863, a man named Friedrich Kasiski proposed a method of determining the key length within a Vigenère Cipher by measuring the difference between numerical locations of patterns within the ciphertext. For example, if one is to note the pattern IQE at location 110, 138, and 226, it is likely that the keyword length divides the difference between these locations. The differences 138 - 110 = 28 and 226 - 110 = 116 leave us with two non-prime numbers, which in turn are factored into primes as $2^2 * 7$ and $2^2 * 29$. This suggests that the length of the key is equal to $2^2$, or 4. The ciphertext is then blocked into lengths equal to the keylength, and characters under each component of the key are examined on their own utilizing frequency analysis. From this frequency analysis, it is possible to determine what the key is, as the Vigenère Cipher has been reduced into a monoalphabetic cipher. Means to avoid this type of analysis were developed, such as the running key Vigenère Cipher, which utilizes a key that is at least the same length as the plaintext that it will be utilized to encrypt. This makes the Kasiski analysis impossible, however, given a long enough ciphertext, patterns within the key may show themselves. To avoid this, a key comprised of totally random characters that is at least the length of the cleartext is utilized, which makes the ciphertext immune to known cryptanalysis. This is called a Vernam One-Time Pad, and is a perfectly valid means of symmetric cryptography.

## Feistel Networks

Feistel networks are the conclusion of the "Classical Cryptography" portion of this writeup. A Feistel Network is a $\mathit{block}\\ \mathit{cipher}$, which utilizes blocks of cleartext of a fixed size. Feistel Networks operate via $\mathit{confusion}$ and $\mathit{diffusion}$. Confusion is the goal of confusing the relationship between the key and the ciphertext, and diffusion is the goal of making the ciphertext bit depend on multiple bits of plaintext data. A Feistel Cipher maps $2n$ bits to a ciphertext of the same length,
with a key of length $n$, where $n \in \mathbb{N}$. Some function F, typically a XOR, must also be defined. Once this has been determined, a number of rounds, key, and cleartext are passed to the Feistel network, where the number of rounds is the number of iterations that a Feistel Network will go through, the key is some key $k$ of length $n$, 
and the cleartext is the original message $m$.

At this point, the Feistel Nework functions as follows.

$E_{k}$ is defined as:
- Let i be defined as 0,1,...,r-1,r
- Let $L_{i}$ and $R_{i}$ be the Left and Right half of the bitstring being processed, with that being initialized as the cleartext such that $L_{0}$ is the left half of the cleartext and $R_{0}$ is the right half of the cleartext.
- For each round r, calculate:
- $L_{i+1} = R_{i}$
- $R_{i+1} = L_{i} \bigoplus F(R_{i},K_{i})$

Which yields a ciphertext C = $R_{r+1}$ | $L_{r+1}$.

$D_{k}$ is defined as:
- Let i be defined as r,r-1,...,1,0
- Let $R_{i+1}$ be initialized as the $\mathit{left}$ half of the ciphertext C, and $L_{i+1}$ be initialized as the $\mathit{right}$ half of the ciphertext C
- For each round r, calculate:
- $R_{i} = L_{i+1}$
- $L_{i} = R_{i+1} \bigoplus F(L_{i+1},K_{i})$

Which yields the plaintext message M. 

Feistel Networks are also able to utilize unbalanced $L$ and $R$ subsets with a modified structure, however, this is the generic form for which they take.

### Why Don't We Use It?

We do! While DES, the most well-known implementation of Feistel Networks, is no longer in use, it is not due to the Feistel Network itself. Feistel Networks, and modified Feistel Networks, are still in use with numerous protocols, such as Twofish, Camellia, and many others. From this point forward, all algorithms mentioned are in active use in modern cryptographic protocols.

## More Background

From this point forward, some additional background in Abstract Algebra and Number Theory must be provided. If you are already comfortable in these fields, please feel free to skip ahead to [RSA](#rsa).

We must first define one of the foundations for the rest of the writeup, which is an algebraic structure known as a $\mathit{Group}$. A group is a set of numbers, defined under some operation, denoted here as $\bullet$, such that the following $\mathit{axioms}$, or rules, are satisfied:

- Associativity: $\forall a,b,c \in G, a \bullet (b \bullet C) = (a \bullet b) \bullet c$
- Identity: $\exists i \in G : \forall a \in G, i \bullet a = a, a \bullet i = a$.
- Inverse: $\forall a \in G, \exists b : a \bullet b = i, b \bullet a = i$, where $i$ is the identity element.
- Closure: $\forall a,b \in G, \exists c \in G : a \bullet b = c$.

We must also define the term $\mathit{Abelian}\\ \mathit{group}$, which adds the axiom of commutativity, such that:
- $\forall a,b \in G, a \bullet b = b \bullet a$.

Groups have or may have certain properties, such as:
- Order: The number of elements within the group. A group must have some order, represented $ord(G)$ or $|G|$.
- Generators: An element $a \in G$ such that $a^n$ enumerates all elements of $G$.

We are interested in two kinds of groups in this writeup:
- Groups called $\mathbb{Z}_{p}^*$ groups, which consist of all numbers coprime with p, which in turn means that they share no common factors other than 1.
- Groups formed with Elliptic Curves under a special definition of addition.

In terms of generators, we are interested in finding the order of elements in $\mathbb{Z}_{p}^*$:


Given the order of $\mathbb{Z}_{p}^* = n$, we want to find $z : z^n \equiv 1$.

Examples of $\mathbb{Z}_{p}^*$ groups are as follows:
- $\mathbb{Z}_{8}^* = \\{1,3,5,7\\}$
- $\mathbb{Z}_{7}^* = \\{1,2,3,4,5,6\\}$

You may notice that $|\mathbb{Z}_{7}^*| = p - 1 \rightarrow 7 - 1 = 6$. This is true for all groups of this nature with $p \in primes$.

We must also bring in the problem of $\mathit{P}$ vs $\mathit{NP}$, which postulates whether or not that each problem with a solution that is easily verified as valid also possesses a solution which is computationally "easy" to calculate. Computationally "easy" is defined as solvable within polynomial time. Cryptography, as a field, is predicated upon this being not true, however, it is still an open question. ([Solve it, and win $1,000,000 USD!](https://brilliant.org/wiki/millennium-prize-problems/#p-vs-np)).

Given this, we must introduce two problems, that, as of the time of writing, are currently not solveable in polynomial time, however, are easily verified to be true.
- Integer factorization. RSA utilizes two large integers, $p$ and $q$, then multiplies them together to form $n \in $\mathbb{N}$ such that the only factors of $n$ are, indeed, $p$ and $q$. It then tasks the eavesdropper with factoring $n$ in order to recover the cleartext from the ciphertext.
- The Discrete Logarithm. Given a group $G$, let $a,b \in G$. The discrete logarithm problem asks the eavesdropper to find $k \in \mathbb{Z} : b^k = a$. This is the problem utilized by ElGamal. 

Lastly, we will discuss Euler's Totient Function, which has the notation $\phi(n)$. This is the number of numbers coprime with $n$. Euler's Totient Function is defined as:
- $\phi(n) = n\prod_{p|n}\left( 1-\frac{1}{p} \right)$

In English, this simply means, for each prime number that divides $n$, multiply $\left( 1-\frac{1}{p} \right)$, with $p$ equal to some distinct prime that divides $n$.

It is of note that if $n$ is prime, $\phi(n) = (n-1)$. This is a multiplicative property, such that if $n,a$ are coprime, $\phi(na) = \phi(n) * \phi(a) = (n-1)(a-1)$.

Thus concludes our essential bits from Abstract Algebra and Number Theory. Now back to the cryptography!

## RSA

RSA, or Rivest-Shamir-Adleman, is an asymmetric-key cryptography algorithm, also known as a public-key cryptography algorithm. Public key cryptography works via a publication of a secret key by a user, such that anyone may use this public key to encrypt their message $M$ into $C$ and send it to the recipient. However, only the holder of the private key may decrypt this ciphertext $C$ back into $M$. RSA publishes public keys in the manner ($n$,$e$),
such that $n$ = $p * q$, and $e$ is the $\mathit{encryption}\\ \mathit{exponent}$, or public key, which has already been previously computed by the individual who has published the public key. The private key of the individual is thus $d$, the $\mathit{decryption}\\ \mathit{exponent}$, also known as the private key. Something to note is that $\forall e, \exists d : ed\\ \mathbf{mod}\\ \phi (pq) \equiv 1$. Therefore, in order to ascertain $d$, one must first factor $n$. There is no known non-quantum (more on this later) efficient algorithm for this task, leading to the RSA Problem of large integer factorization.

$E_{k}$ for RSA is as follows:
- Let $p,q$ be sufficiently large prime numbers. Let $n = p * q$. $p$ and $q$ should be kept secret.
- Take $\phi(pq) = (p-1)(q-1) = \phi$. Choose an encryption exponent, $e$, such that $2 < e < \phi$, and $e$ is coprime with $\phi$.
- Determine $d$ as $d \equiv e^{-1}$
- Publish ($n$, $e$), keeping $d$ private.

### Determining $d$
Some of you may be scratching your heads, asking yourselves why we are able to trivialize finding the inverse for the encryption exponent when it would seem that $de \bmod \phi \equiv 1$ is an incredibly annoying problem for large numbers. Fear not! This can be accomplished using the
$\mathit{Extended}\\ \mathit{Euclidean}\\ \mathit{Algorithm}$, described as follows:
- Let $n$ be the number of rows, $x_{i}$ be a factor you must solve for, $r_{i}$ be the remainder given $x_{i}$, and $s_{1}, s_{2}, s_{3}$ be columns to the right. Set $r_{1}$ as $\phi = x_{1} * e + r_{1} | $s_{1} = 0, s_{2} = 1, s_{3} = s_{1} - x_{1} * s_{2}$
- This results in $\phi = x_{1} * e + r_{1} |$ ($0$) ($1$) ($0-x_{1}*1$)
- Upon computing this, generate $row_{2}$ as $e = x_{2} * r_{1} + r_{2}  |$ ($1$) ($0-x_{1} * 1$) ($1 - x_{2} * s_{2}$) (Please note, the reference to $s_{2}$ here refers to the value of $s_{2}$ on $row_{2}$, not $row_{1}$. 
- This pattern is repeated until $r_{n} = 0$, at which point the value in $s_{2}$ on $row_{n}$ is $\equiv e^{-1} \bmod \phi$.

$D_{k}$ for RSA is quite simple, and is as follows:
- $c^d \bmod n \equiv m \bmod n$


The RSA numbers, which can be found [here](https://en.wikipedia.org/wiki/RSA_numbers), are the ones utilized in legitimate implementations.
Legitimate RSA implementations also utilize padding, as there have been several successful, although highly specific, attacks against the protocol without it. However, with padding, RSA is currently regarded as secure.

## ElGamal




