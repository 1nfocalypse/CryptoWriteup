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
<p align="center">
  An Writeup on Mathematical Cryptography, from Shift Ciphers to Elliptic Curve Groups
</p>


## Contents

- [Summary](#summary)
- [Some Background](#background)
- [Shift Ciphers](#classical)
- [Vigenère Cipher](#classical)
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

This document is intended to serve as a reference guide along with a resource for mathematical cryptography. While it may be useful to utilize it to supplement for self study, it is not intended to replace an appropriate curriculum. This document is also not meant to be a comprehensive guide to cryptography, and as such, not all systems or features will be talked about, i.e. NTRU, signatures, etc. There will, however, be recommended reading delineated at the bottom of this guide that will be much more comprehensive. This guide assumes knowledge of algebra and some number theory, however, anything past high school level will be explained, albeit with some brevity. It is highly encouraged to pursue study in these areas yourself for furter information. Most informationation is sourced from [Coding Theory and Cryptography: The Essentials 2Ed](https://www.amazon.com/Coding-Theory-Cryptography-Essentials-Mathematics/dp/0824704657/), Wikipedia, and personal notes from a university mathematics course. Some of the contents here either have or will have toy implementations of them in C++ on my main profile. These projects are ongoing, and as such may not be completed or fully functional by the time of reading. However, if they are, please feel free to look at and play around with them.
