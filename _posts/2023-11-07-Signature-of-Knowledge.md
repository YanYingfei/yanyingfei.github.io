---
layout: post
title: Signature of Knowledge
categories: signatures
author: Yingfei
---

In a traditional signature scheme, a signature $\sigma$ on a message $m$ is issued under a public key $PK$, and can be interpreted as follows: “The owner of the public key $PK$ and its corresponding secret key has signed message $m$.” In the signature of knowledge, we consider schemes that allow one to issue signatures on behalf of any NP statement, that can be interpreted as follows: “A person in possession of a witness $w$ to the statement that $x \in L$ has signed message $m$.” <!--more-->

Yesterday I discussed the notion with my friend. Below I would like to record the discussion. 

### Definitions

*Definition (Signature of Knowledge, SoK).*
Let $\mathcal{R}$ be a relation generator and let $$\{M_\lambda \}_{\lambda \in \mathbb{N}}$$ be a sequence of message spaces. Then a SoK scheme consists of 3 algorithms: ($\mathsf{Setup}$, $\mathsf{Sign}$, $\mathsf{Verify}$).
- $\mathsf{pp} \gets \mathsf{Setup} (R)$: This algorithm is a PPT algorithm which takes as input a relation $R \in \mathcal{R}_\lambda$ and returns public parameters $\mathsf{pp}$.
- $\sigma \gets \mathsf{Sign}(\mathsf{pp},x, w, m):$ the signing algorithm is a PPT algorithm which takes as input the public parameters, a pair $(x,w) \in M_L$ and a message $m \in M_\lambda$ and returns a signature $\sigma$.
- $0/1 \gets \mathsf{Verify}(\mathsf{pp}, x, m, \sigma)$: the verification algorithm is a deterministic polynomial time algorithm, which takes as input some public parameters $\mathsf{pp}$, an instance $x$, a message $m \in M_\lambda$, and a signature $\sigma$ and outputs a 1 or a 0 that indicates the signature is valid or not.

#### Functionality for a SoK
Chase and Lysyanskaya [<a href="#ref1">1</a>] give two ways to define the security of a SoK: UC framework and security game. 

First, we give security in the UC model. We need to define two functionalities.

The signature functionality is defined in the following figure. 

<p><img src="/functionality-sig.png" alt="functionality-sig"></p>
  
where the first messages $KeyGen$, $Sign$, and $Verify$ of each tuple indicate the name of the request,  and the last messages $\mathsf{Sign}$ and/or $\mathsf{Verify}$ are the algorithms (ITM, Involutory Turing Machines, $f(f(x))=x$). 

For honest signers and verifier, the functionality generates the correct transcripts for both parties. 

As for the adversary, the functionality captures the correctness and unforgeability game. Concretely, the adversary can: 
1. get access to the $\mathsf{KeyGen}$ algorithm,
2. find message $m$ such that the according signature is invalid, ie.,$\mathsf{Verify}(m, \sigma) = 0$, to break the correctness,
3. forge a signature without the signing key to break the unforgeability.

The ideal signature functionality captures the above items in $\mathsf{KeyGen}$, $\mathsf{Sign}$ and $\mathsf{Verify}$, respectively.

Similarly, the functionality of SoK is defined as follows:

<p><img src="/functionality-sim-sok.png" alt="functionality-sok"> </p>


The setup procedure outputs algorithms $\mathsf{Simsign}$ and $\mathsf{Extract}$ for the adversary.
- $\sigma' \gets \mathsf{Simsign}(\mathsf{pp}, x, \tau, m)$: This PPT algorithm takes the the public parameter $\mathsf{pp}$ (sometimes implicitly), the instance $x$, a trapdoor $\tau$ (optionally) and a message $m$ as input and outputs a simulated signature $\sigma'$.
- $w' \gets \mathsf{Extract} (\mathsf{pp}, x, m, \sigma)$ 

There are three things that the *signature generation* part of the $F_{SOK}$ functionality captures. 
- **(Soundness)** The first is that to issue a signature, the party who calls the functionality must supply $(m, x, w)$ where $w$ is a valid witness to the statement that $x \in L$. 
- **(Zero-knowledge)** The second is that a signature reveals nothing about the witness that is used. This is captured by issuing the formal signature $\sigma$ via a procedure ($\mathsf{Simsign}$) that does not take $w$ as an input.
- **(Correctness)** Finally, the signature generation step must ensure that the verification algorithm $\mathsf{Verify}$ is complete, i.e., that it will accept the resulting signature $\sigma$. If it finds that $\mathsf{Verify}$ is incomplete, $F_{SOK}$ will output an error message (Completeness error) and halt, just as $F_{SIG}$ does.

The *signature verification* part of $F_{SOK}$ accepts signatures $(m, x, \sigma)$ such that $\mathsf{Verify}(m, x, \sigma) = 1$ for $x \in L$, which is captured by two aspects. 
- **(Unforgeability)** $m$ was previously signed on behalf of $x \in L$, and $\sigma$ is the resulting signature. Namely, $(m, \sigma)$ is recorded in $\mathsf{Sign}$, which is the same as in $F_{SIG}$.
- **(Knowledge Soundness)** When $m$ was not signed previously, the verification algorithm must somehow check that whoever generated $\sigma$ knew the witness $w$. That is, the algorithm can run $\mathsf{Extract}$ to extract the witness $w$ behind a signature $(m,\sigma)$. If the resulting witness $w$ is valid, then the signature is valid.

#### Game Style.
In this section, we show the game-based security. In a word, a SoK is SimExt-secure if it is correct, simulatable, and sim-extractable. The formal definition is given below.

*Definition (SimExt-security).* Let $L$ be the language defined by a polynomial-time Turing machine $M_L$ as explained above, such that all witnesses for $x \in L$ are of known polynomial length $p(\vert x \vert)$. Then ($\mathsf{Setup}$, $\mathsf{Sign}$, $\mathsf{Verify}$) constitute a SimExt-secure signature of knowledge of a witness for $L$, for message space $\{ M_\lambda \}$ if the following properties hold:

- **Correctness**: There exists a negligible function $\nu$ such that for all $x \in L$, valid witnesses $w$ for $x$ (i.e. witnesses $w$ such that $M_L(x,w) =1$) and $m \in M_\lambda$ 
$\begin{equation} Pr[\mathsf{pp} \gets \mathsf{Setup}(R); \sigma \gets \mathsf{Sign}(\mathsf{pp},x, w, m):  \mathsf{Verify}(\mathsf{pp}, x, m, \sigma) = 1] = 1 - \nu{\lambda} \end{equation}$

- **Simulatability** There exists a polynomial time simulator consisting of algorithms $\mathsf{Simsetup}$ and $\mathsf{Simsign}$ such that for all probabilistic polynomial-time adversaries $\mathcal{A}$ there exists a negligible functions $\nu$ such that for all polynomials $f$, for all $\lambda$, for all auxiliary input $$s \in \{0,1\}^{f(\lambda)},$$
\begin{equation} \bigg| \begin{aligned} Pr[(\mathsf{pp},\tau) \gets \mathsf{Simsetup}(\lambda); b\gets \mathcal{A}^{\mathsf{Simsign}(\mathsf{pp}, \tau,\cdot)}(s,\mathsf{pp}): b=1] \\\\ - Pr[\mathsf{pp} \gets \mathsf{Setup}(\lambda); b\gets \mathcal{A}^{\mathsf{Sign}(\mathsf{pp}, \cdot)}(s,\mathsf{pp}): b=1]\end{aligned} \bigg|= \nu(\lambda), \end{equation}
where $\tau$ is the additional trapdoor value that the simulator needs to simulate the signatures without knowing a witness.

- **Sim-Extraction** In addition to $(\mathsf{Simsetup, Simsign})$, there exists an extractor algorithm $\mathsf{Extract}$ such that for all probabilistic polynomial time adversaries $\mathcal{A}$ there exists a negligible functions $\nu$ such that for all polynomials $f$,  for all $\lambda$, for all auxiliary input $$s \in \{0,1\}^{f{(\lambda)}},$$ 
\begin{equation} Pr\left[\begin{aligned} (\mathsf{pp},\tau) \gets \mathsf{Simsetup}(\lambda); (M_L,x,m,\sigma) \gets \mathcal{A}^{\mathsf{Simsign}(\mathsf{pp}, \tau,\cdot)}; \\\\\\\\
w \gets \mathsf{Extract}(\mathsf{pp}, \tau, M_L, x, m, \sigma):\\\\\\\\
M_L(x,w) \vee (M_L, x, m) \in Q^{+} \vee \lnot \mathsf{Verify}(\mathsf{pp},M_L, x, m, \sigma) \end{aligned} \right] = 1 - \nu(\lambda) \end{equation}
where $Q^+$ denotes the query tape which lists all the previous successful queries $\mathcal{A}$ has sent to the oracle $\mathsf{Simsign}$, i.e. all those queries $(M_L, x, m)$ which were sent with some valid witness $w$.

Simulatability captures the zero-knowledge property, that is, the resulting signature $\sigma$ does not leak the witness $w$. 

Meanwhile, Sim-Extraction (SE) captures soundness (of a proof) and unforgeability (of a signature). 
- **Unforgeability**. Given the access to the signing orcale, the unforgeability requires that, without a signing key, an adversary cannot produce a valid signature. As we are talking about the SoK, the signing key is replaced by the witness $w$. Namely, either the output signature is invalid with high probability or the message is queried previously: \begin{equation} (M_L, x, m) \in Q^{+} \vee \lnot \mathsf{Verify}(\mathsf{pp},M_L, x, m, \sigma) = 1, \end{equation}
- **Soundness**. Consider that the adversary outputs a valid signature (breaks the unforgeability). With high probability, then we can extract the witness $w$ of the instance $x$ such that $M_L(x,w) = 1$.

#### Why Sim-ext?

One can notice in the game sytle definition, the adversary is allowed to access a **sim-signing oracle**, while in a digital signature scheme, the adversary accesses to the real signing oracle. There are two ways to understand the sim-signing oracle. 
- In a digital signature scheme, only the one with signing key (signer) can generate the valid signature. However in the SoK, anyone holds a witness $w$ to the statement $x$ can produce the signature. If we define an orcale that has the witnesses for all the statements, then such an oracle seems too powerful. 
- As the output of simsign and sign are distinguishable, which is garuanteed by *simulatability*, it's safe to replace the signing oracle by sim-signing oracle. This is like we usually do in the secuirty proof of the unforgeability: construct a simulator that answers the signing queries without a signing key and prove the answer of simulator is indistinguish from that of a signing oracle.

### The Construction
I mixed the security notions of NIZK and SoK many times when introducing the deifinitions. In this part, I would like to show two constructions of SoK.

I use the word "soundness" in the previous section. Actually, it is not accurate, only for understanding the notions, and should be "simulation extractability": 

- **Simulation extractability** requires that one be able to extract a witness underling a valid proof even when the statement was created after seeing simulated proofs.

This leads to the first construction [<a href="#ref2">2</a>], which applies an SE-NIZK (sim-ext non-interactive zero-knowledge argument) $\Pi = (\mathsf{Z.Setup, Z.Prove, Z.Verify})$ and a collision-resistant hash-function $H(\cdot)$.  I describe the construction as follows.

- $\mathsf{pp} \gets \mathsf{S.Setup}(\lambda):$  
	1. $(crs, \tau) \gets \mathsf{Z.Setup}(\lambda).$
	2. return $\mathsf{pp} = crs$.
- $\sigma \gets \mathsf{S.Sign}(\mathsf{pp},x, w, m):$
	1. $ K \gets \{0,1\}^{\ell(\lambda)}. $
	2. $\pi \gets \mathsf{Z.Prove} (crs, (K, H_K(m), x), w).$
	3. return $\sigma = (K, \pi).$
- $\mathsf{S.Verify}(\mathsf{pp}, x, m, \sigma)$
	1. return $\mathsf{Z.Verify}(crs, (K, H_K(m), x), \pi).$

In the above scheme, SE-NIZK $\Pi$ proves the relation: 
$$\begin{equation} R=\{((K, h, x); w) : K \gets \{0,1\}^{\ell(\lambda)} \land h \gets \{0,1\}^{\ell(\lambda)} \land (x,w) \in M_L \}. \end{equation}$$

- $\mathsf{Simsetup}:$
	1. $(crs, \tau) \gets \mathsf{Z.Setup}(\lambda).$
	2. return $\mathsf{pp} = (crs, \tau)$.
- $\mathsf{Simsign}:$
	1. $ K \gets \{ 0,1 \}^{\ell(\lambda)}.$
	2. $\pi \gets \mathsf{Z.Simprove} (crs, \tau,(K, H_K(m), x)).$
	3. return $\sigma = (K, \pi).$

The second construction applies CPA secure dense encryption scheme $\mathsf{(KeyGen, Enc, Dec)}$ and simulation-sound non-interactive zero knowledge proofs $\Pi = (\mathsf{Z.Setup, Z.Prove, Z.Verify})$. I describe the construction as follows.

- $\mathsf{pp} \gets \mathsf{S.Setup}(\lambda):$  
	1. $PK \gets \mathsf{KeyGen}(\lambda).$
	2. $(crs, \tau) \gets \mathsf{Z.Setup}(\lambda).$
	3. return $\mathsf{pp} = (PK, crs)$.
- $\sigma \gets \mathsf{S.Sign}(\mathsf{pp},x, w, m):$
	1. sample $r \gets R$ from a randomness space.
	2. $c=\mathsf{Enc}(PK,(m,w),r).$
	3. $\pi \gets \mathsf{Z.Prove} (crs, (m, M_L, x,c,PK), w).$
	4. return $\sigma = (c, \pi).$
- $\mathsf{S.Verify}(\mathsf{pp}, x, m, \sigma):$
	1. return $\mathsf{Z.Verify}(crs, (m, M_L, x,c,PK), \pi).$

In the scheme, $\Pi$ proves the relation: 

$\begin{equation} R= \{ ((m, M_L, x,c,PK); w) : c=\mathsf{Enc}(PK,(m,w),r) \land (x,w) \in M_L \}. \end{equation}$

The security proofs refer to the corresponding literature.

---
1. <p name = "ref1"> Melissa Chase and Anna Lysyanskaya. On Signature of Knowledge. Cryptology ePrint Archive, Paper 2006/184.</p>

2. <p name = "ref2"> Jens Groth and Mary Maller. Snarky Signatures: Minimal Signatures of Knowledge from Simulation-Extractable SNARKs. Cryptology ePrint Archive, Paper 2017/540. </p>
