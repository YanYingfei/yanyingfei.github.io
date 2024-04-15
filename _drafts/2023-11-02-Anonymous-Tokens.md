---
layout: post
title: Anonymous Tokens
categories: advanced protocols
author: Yingfei
---

Inspired by privacy pass, anonymous token enables an issuer to provide a user with a lightweight, single-use anonymous trust token that can embed a single private bit, which is accessible only to the party who holds the secret authority key and is private with respect to anyone else. <!--more-->

This time I would like to talk about the recent progress on this primitive. I will go through its related works, including the literatures [<a href="#ref1">1</a>, <a href="#ref2">2</a>, <a href="#ref3">3</a>, <a href="#ref4">4</a>, <a href="#ref5">5</a>, <a href="#ref6">6</a>, <a href="#ref7">7</a>], and discuss about the syntax and the main methods of construction.

### Category

Anonymous token (Privacy Pass)  [<a href="#ref7">7</a>] was designed to distinguish honest from malicious content requests in content delivery networks (CDNs), so as to block illegitimate traffic that could drain network resources causing a denial of service (DoS). 

The privacy pass protocol [<a href="#ref7">7</a>] can be regarded as blind signature (token signing protocol) + special verification(token redemption protocol). In the token signing protocol, the client and the server interacts as in a blind signature, while in the token redemption protocol, the client sends out a MAC message with respect to the blind signature and the server redeems the token by verifying the MAC. We can notice that the privacy pass didn't go far from the blind signature.

Later on, the privacy pass is extended to that with a single private bit: Anonymous Tokens with Private Metadata Bit, by Kreuter et al in Crypto 2020 [<a href="#ref1">1</a>]. Following [<a href="#ref7">7</a>], their construction applies VOPRF. In this syntax, the anonymous tokens can convey two trust signals that the client cannot distinguish which of the two signals is embedded in her tokens. This extension avoids the attack that in a system relying on anonymous trust tokens, malicious users be identified as a threat if the issuer stops providing them with tokens. 

Next, in FC22, anonymous tokens was developed to including public metadata and public verification[<a href="#ref2">2</a>]. Meanwhile, the authors give the corresponding constructions using VOPRF technique. Thus we have the following 6 categories.

1. With designated verification:
	1. Anonymous single-use tokens
	2. Anonymous single-use tokens with private metadata bit
	3. Anonymous single-use tokens with public metadata
	4. Anonymous single-use tokens with public and private metadata
2. With public verification:
	1. Anonymous single-use tokens
	2. Anonymous single-use tokens with public metadata

*single-use*： A token can only be verified once. A second-time usage will break the security.

Later in the literature [<a href="#ref3">3</a>],  the authors apply 'one-more DL' assumption and achieve publicly verifiable anonymous tokens with private metadata bit.

crypto23[<a href="#ref4">4</a>]

Then in Asiacrypt23 [<a href="#ref5">5</a>] , a new primitive called anonymous counting token is proposed to limit the 



### Syntax

### Main Constrcutions
#### Privacy Pass

The privacy pass protocol applies a main building block: verifiable 'oblivious pseudorandom function' (OPRF), which is similar to a blind-RSA interation but more efficient (less than 1-RTT). 

#### Anonymous Token with Private Bit
crypto22 crypto23

#### Other Constructions
counting tokens
rsa, pairing

---
1. <p name = "ref1"> Ben Kreuter, Tancrède Lepoint, Michele Orrù and Mariana Raykova. Anonymous Tokens with Private Metadata Bit. Cryptology ePrint Archive, Paper 2020/072.</p>
2. <p name = "ref2"> Tjerand Silde and Martin Strand. Anonymous Tokens with Public Metadata and Applications to Private Contact Tracing. Cryptology ePrint Archive, Paper 2021/203.</p>
3. <p name = "ref3"> Fabrice Benhamouda, Tancrède Lepoint, Michele Orrù and Mariana Raykova. Publicly verifiable anonymous tokens with private metadata bit. Cryptology ePrint Archive, Paper 2022/004.</p>
4. <p name = "ref4"> Fabrice Benhamouda, Mariana Raykova and Karn Seth. Anonymous Counting Tokens, Cryptology ePrint Archive, Paper 2023/320.</p>
5. <p name = "ref5"> Melissa Chase, F. Betül Durak and Serge Vaudenay. Anonymous Tokens with Hidden Metadata Bit from Algebraic MACs. Cryptology ePrint Archive, Paper 2022/1622.</p>
6. <p name = "ref6"> Ghous Amjad and Kevin Yeo and Moti Yung. RSA Blind Signatures with Public Metadata. Cryptology ePrint Archive, Paper 2023/1199.</p>
7. <p name = "ref7">Alex Davidson, Ian Goldberg, Nick Sullivan, George Tankersley, and Filippo Valsorda. Privacy Pass: Bypassing Internet Challenges Anonymously. Proc. Priv. Enhancing Technol. 2018(3), 164–180 (2018).</p>
