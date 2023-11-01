---
layout: post
title: "Arithmetisation: CCS"
categories: snark
author: Yingfei
meta: "Springfield"
---

Recently, I read the new paper KiloNova [<a href="#ref1">1</a>], where the authors propose a PCD scheme based on the CCS relation. Before KiloNova, I would like to list the representations of arithmetic circuits: CCS (and its variants), R1CS, Plonkish and AIR.

## Customizable Constraint Systems

The customizable constraint cystems are proposed by Srinath Setty et al in [<a href="#ref2">2</a>] to generalize the different arithmetizations.  Here is the formal definiation of CCS relations.

### CCS
*Definition (CCS)*. An $$\mathcal{R}_{CCS}$$ structure $$\mathcal{S}$$ consists of:
- size bounds $$m,n,N,\ell,t,q,d \in \mathbb{N}$$, where $$n>\ell$$.
- a sequence of matrices $$\{M_j \in \mathbb{F}^{m\times n}\}_{j\in[t]}$$ with at most $$N = \Omega(\max(m,n))$$ non-zero enrtiyes in total.
- a sequence of $$q$$ multisets $$\{S_i\}_{i\in [q]}$$ , where the elements in each multiset $$S_i$$ is from $$\{1, \dots, t\}$$ and the cardinality of each multiset is at most $$d$$.
- a sequence of q constants $$\{c_i\}_{i∈[q]}$$, where each constant is from $$\mathbb{F}$$.
An $$\mathcal{R}_{CCS}$$ instance (structure-context tuple $$(\mathcal{S}, \mathsf{io})$$ is satisfied by an  $$\mathcal{R}_{CCS}$$ witness $$w \in \mathbb{F}^{n-\ell-1}$$ and the public input and output $$\mathsf{io} \in \mathbb{F}^\ell$$, if
$$\sum_{i\in[q]} c_i~ \circ_{j \in S_i}~ (M_j \cdot \vec{z}) = \vec{0}, \tag{1}$$
where $$z = (w, 1, \mathsf{io}) ∈ \mathbb{F}^n$$, $$M_j · \vec{z}$$ denotes matrix-vector multiplication, $$\circ$$ denotes the Hadamard product between vectors, and $$\vec{0}$$ is an $$m$$-sized vector with entries equal to the additive identity in $$\mathbb{F}$$.

### R1CS
*Definition (R1CS)*. An R1CS structure $$\mathcal{S}$$ consists of size bounds $$m,n,N,\ell$$, where $$n>\ell$$ and three matrices $$A,B,C\in \mathbb{F}^{m \times n}$$ with at most $$N = \Omega(\max(m,n))$$ non-zero entries in total. 
An R1CS instance consists of public input and output $$x \in \mathbb{F}^\ell$$. An R1CS witness consists of a vector $$w \in \mathbb{F}^{n-\ell-1}$$. 
An R1CS structure-instance tuple $$(\mathcal{S}, x)$$ is satisfied by an R1CS witness $$w$$ if 
$$(A \cdot \vec{z}) \circ (B \cdot \vec{z}) - C \cdot \vec{z} = \vec{0} \tag{2},$$
where $$\vec{z} = (w,1,x) \in \mathbb{F}^n$$, $$()· \vec{z}$$ denotes matrix-vector multiplication, $$\circ$$ denotes the Hadamard product between vectors, and $$\vec{0}$$ is an $$m$$-sized vector with entries equal to the additive identity in $$\mathbb{F}$$.

Following the above definitions, one can easily do the transformation from $$R1CS$$ to $$CCS$$ relations.

### Plonkish
Below I put the definition of Plonkish from [<a href="#ref2">2</a>]. 

*Dfinition (Plonkish).* A Plonkish structure $$\mathcal{S}$$ consists of: 
- size bounds $$m, n, \ell, t, q, d, e, \in \mathbb{N}$$;
- a multivariate polynomial $$g$$ in $$t$$ variables, where $$g$$ is expressed as a sum of $$q$$ monomials and each monomial has a total degree at most $$d$$;
- a vector of constants called selectors $$s \in \mathbb{F}^e$$;
- a set of $$m$$ constraints. Each constraint $$i$$ is specified via a vector $$T_i$$ of length $$t$$, with entries in the set $$[n+e-1]$$. $$T_i$$ is interpreted as specifying $$t$$ entries of a purported satisfying assignment $$\vec{z}$$ to feed into $$g$$.
A Plonkish instance consists of public input and output $$x \in \mathbb{F}^\ell$$. A Plonkish witness consists of a vector $$w \in \mathbb{F}^{n-\ell}$$. A Plonkish structure-instance tuple $$(\mathcal{S}, x)$$ is satisfied by a Plonkish witness $$w$$ if:
$$\forall i\in[m], g(z[T_i[1]], \dots, z[T_i[t]]) = 0,$$
where $$\vec{z} = (w, x, s) \in \mathbb{F}^{n+e}.$$

Generally, PLONK's arithmetization [<a href="#ref3">3</a>] (Plonkish) has two main components, namely, the gate constraints with respect to a vector of selectors $$s$$ and the copy constraints with repsect to a permutation $$\varphi$$. 

In the above definition, the copy constraints are replaced by ''deduplicated'' gate constraints. For example, we have a copy constraint  $$c_0 = a_1$$ and a gate constraint $$g_0$$ with resprect to $$c_0$$. To represent this equation, we define a new gate constraint $$g_0'$$ by "deduplicating" the gate constraint $$g_0$$ and replacing $$c_0$$ with $$a_1$$.

### AIR
*Dfinition (AIR).* A AIR structure $$\mathcal{S}$$ consists of: 
- size bounds $$n, q, d \in \mathbb{N}$$ and an even $$t$$;
- a multivariate polynomial $$g$$ in $$t$$ variables, where $$g$$ is expressed as a sum of $$q$$ monomials and each monomial has a total degree at most $$d$$.
An AIR instance consists of public input and output $$x \in \mathbb{F}^t$$, and the witness consists of a vector $$w \in \mathbb{F}^{(m-1)\cdot t/2}$$.  
Parse $$x=(x_0, x_1)$$ and set $$\vec{z} = (x_0, w, x_1) \in \mathbb{F}^{(m+1)\cdot t/2}$$, where $$x_0, x_1 \in \mathbb{F}^{t/2}$$. Parse $$z= (z_0, \dots, z_m)$$, where each $$z_i \in \mathbb{F}^{t/2}$$ for $$i \in [m+1]$$.  
An AIR structure-instance tuple $$(\mathcal{S}, x)$$ is satisfied by the witness $$w$$ if:
$$\forall i \in \{1,\dots, m\}, g(z_{i-1}, z_i, z_{i+1}) = 0.$$

---
1. <p name = "ref1"> Tianyu Zheng, Shang Gao, Yu Guo and Bin Xiao, KiloNova: Non-Uniform PCD with Zero-Knowledge Property from Generic Folding Schemes, Cryptology ePrint Archive, Paper 2023/1579.</p>
2. <p name = "ref2"> Srinath Setty, Justin Thaler and Riad Wahby, Customizable constraint systems for succinct arguments, Cryptology ePrint Archive, Paper 2023/552.</p>
3. <p name = "ref3"> Ariel Gabizon and Zachary J.  Williamson, plookup: A simplified polynomial protocol for lookup tables, Cryptology ePrint Archive, Paper 2020/315.</p>
---
