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
<h3 align="center">Cryptography</h3>
<h2 align="center">
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
- [RSA](#RSA)
- [ElGamal](#ElGamal)
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

$\mathbf{mod}$ : This is the modulus operator, which can be understood as the remainder when dividing the left number by the right. It may also be represented as %.

## Terms

Let us begin by introducing some terms utilized in mathematical cryptography. We will begin with $\mathscr{A}$, or the $\mathit{alphabet}$ $\mathit{space}$. The alphabet space is defined as a finite set of symbols for communication, such as our alphabet, or the binary set K = {0,1}. 

From this, we will then define the $\mathit{message}$ $\mathit{space}$, $\mathscr{M}$. The message space is a string of symbols from $\mathscr{A}$ that is to be communicated. For example, should we wish to send a string of 8 characters with $\mathscr{A}$ = {0,1}, we would denote $\mathscr{M}$ as $\\{ 0,1 \\} ^8$. 

Lastly, we must define the encryption and decryption functions, and along with them, the $\mathit{encryption}$ $\mathit{space}$, $\mathscr{C}$, and the $\mathit{key}$ $\mathit{space}$ $\mathscr{K}$.

The $\mathit{encryption}$ $\mathit{function}$ $E_{k}$ is any function such that $E_{k} : \mathscr{M} \mapsto \mathscr{C}$, with $\mapsto$ denoting that every value in $\mathscr{M}$ must have a corresponding value in $\mathscr{C}$ that will be the result. 

The $\mathit{decryption}$ $\mathit{function}$ $D_{k}$ is any function such that $D_{k} : \mathscr{C} \mapsto \mathscr{M}$.

The encryption space can be thought of simply as another message space, however, it is after the encryption function has been applied to the original message. The key space can be thought of simply as where the keys utilized in the encryption and decryption functions are stored. 

With this in mind, let us proceed to the first $\mathit{cipher}$, or encryption scheme, the Shift Cipher.

## Shift Ciphers

Shift Ciphers are one of, if not the simplest, kinds of ciphers. Originating with Julius Caesar, and hence sometimes called the $\mathit{Caesar}$ $\mathit{Cipher}$, it generally utilizes $\mathscr{A}$ = {A,B,C,...,Y,Z}. Each value in $\mathscr{A}$ is then mapped to a numeric value X = {0,1,2,...,24,25}. This mapping is also utilized with the key, of which $\mathscr{K}$ = $\mathbb{N}$. Upon deciding upon a key value such that $k \in \mathbb{N}$, the encryption function is $E_{k} = m_{i} + k\\ \mathbf{mod}\\ 26$. As you might expect, the decryption function is $D_{k} = c_{i} - k\\ \mathbf{mod}\\ 26$. This results in the values of each letter being shifted to the right by k % 26 to encrypt, and to the left by k % 26 to decrypt. Clasically, Caesar was known to choose k = 3, and as such, so shall we.

Given the table

Let the $\mathit{cleartext}\, or original message, be "ILIKECRYPTOGRAPHY". Let k = 3. Applying the encryption function, we obtain:
$C_{1} = I + 3 \rightarrow 

