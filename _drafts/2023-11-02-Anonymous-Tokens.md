---
layout: post
title: Anonymous Tokens
categories: advanced protocols
author: Yingfei
---

Inspired by privacy pass, anonymous token enables an issuer to provide a user with a lightweight, single-use anonymous trust token that can embed a single private bit, which is accessible only to the party who holds the secret authority key and is private with respect to anyone else. <!--more-->

This time I would like to discuss about the recent progress on this primitive. I will go through its related works, including the literatures[<a href="#ref1">1</a>,<a href="#ref2">2</a>,<a href="#ref3">3</a>,<a href="#ref4">4</a>,<a href="#ref5">5</a>,<a href="#ref6">6</a>], <a href="#ref7">7</a>], the syntax and the main methods of construction.

### State of Art

Anonymous token (Privacy Pass)  [<a href="#ref7">7</a>] was designed to distinguish honest from malicious content requests in content delivery networks (CDNs), so as to block illegitimate traffic that could drain network resources causing a denial of service (DoS). 

### Syntax

### Constrcutions


---
1. <p name = "ref1"> Ben Kreuter, Tancrède Lepoint, Michele Orrù and Mariana Raykova. Anonymous Tokens with Private Metadata Bit. Cryptology ePrint Archive, Paper 2020/072.</p>
2. <p name = "ref2"> Tjerand Silde and Martin Strand. Anonymous Tokens with Public Metadata and Applications to Private Contact Tracing. Cryptology ePrint Archive, Paper 2021/203.</p>
3. <p name = "ref3"> Fabrice Benhamouda, Tancrède Lepoint, Michele Orrù and Mariana Raykova. Publicly verifiable anonymous tokens with private metadata bit. Cryptology ePrint Archive, Paper 2022/004.</p>
4. <p name = "ref4"> Fabrice Benhamouda, Mariana Raykova and Karn Seth. Anonymous Counting Tokens, Cryptology ePrint Archive, Paper 2023/320.</p>
5. <p name = "ref5"> Melissa Chase, F. Betül Durak and Serge Vaudenay. Anonymous Tokens with Hidden Metadata Bit from Algebraic MACs. Cryptology ePrint Archive, Paper 2022/1622.</p>
6. <p name = "ref6"> Ghous Amjad and Kevin Yeo and Moti Yung. RSA Blind Signatures with Public Metadata. Cryptology ePrint Archive, Paper 2023/1199.</p>
7. <p name = "ref7">Alex Davidson, Ian Goldberg, Nick Sullivan, George Tankersley, and Filippo Valsorda. Privacy Pass: Bypassing Internet Challenges Anonymously. Proc. Priv. Enhancing Technol. 2018(3), 164–180 (2018).</p>
