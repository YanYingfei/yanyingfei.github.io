---
layout: post
title: A note on Latticefold
categories: Lattice SNARKs
author: Yingfei
---
Folding is a recent technique for building efficient recursive SNARKs. Several elegant folding protocols have been proposed, such as Nova, Supernova, Hypernova, Protostar, and others. LatticeFold is the first lattice-based folding protocol based on the Module SIS problem. This folding protocol naturally leads to an efficient recursive lattice-based SNARK and an efficient PCD scheme. LatticeFold supports folding low-degree relations, such as R1CS, as well as high-degree relations, such as CCS. <!--more-->

Months ago, I read the new paper "LatticeFold"[<a href="#ref1">1</a>]. I really appreciate the elegant design of this protocol. Here is my handwritten note.

<p><img src="/post-src/latticefold/latticefold1.png" alt="-1-"></p>
<p><img src="/post-src/latticefold/latticefold2.png" alt="-2-"></p>
<p><img src="/post-src/latticefold/latticefold3.png" alt="-3-"></p>
<p><img src="/post-src/latticefold/latticefold4.png" alt="-4-"></p>
<p><img src="/post-src/latticefold/latticefold5.png" alt="-5-"></p>
<p><img src="/post-src/latticefold/latticefold6.png" alt="-6-"></p>
<p><img src="/post-src/latticefold/latticefold7.png" alt="-7-"></p>
<p><img src="/post-src/latticefold/latticefold8.png" alt="-8-"></p>
<p><img src="/post-src/latticefold/latticefold9.png" alt="-9-"></p>
<p><img src="/post-src/latticefold/latticefold10.png" alt="-10-"></p>
<p><img src="/post-src/latticefold/latticefold11.png" alt="-11-"></p>
<p><img src="/post-src/latticefold/latticefold12.png" alt="-12-"></p>
<p><img src="/post-src/latticefold/latticefold13.png" alt="-13-"></p>
<p><img src="/post-src/latticefold/latticefold14.png" alt="-14-"></p>
<p><img src="/post-src/latticefold/latticefold15.png" alt="-15-"></p>

---
1. <p name = "ref1"> Dan Boneh and Binyi Chen. LatticeFold: A Lattice-based Folding Scheme and its Applications to Succinct Proof Systems. Cryptology ePrint Archive, Paper 2024/257.</p>