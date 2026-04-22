---
layout: post
title: "Folding scheme: Neo and SuperNeo"
categories: snark
author: Yingfei
meta: "Springfield"
---
In Neo and SuperNeo, the prover commits to a CCS witness using a lattice-based commitment scheme with pay-per-bit costs, runs a single invocation of the sum-check protocol over an extension of a small prime field, and achieves plausible post-quantum security under a standard structured lattice assumption (Module-SIS).

Key idea
- sumcheck + normcheck over the finite field (Running sumcheck over rings is expensive)
- Commit the witness using Ajtai over ring
- from Fq to Rq: coefficient embedding
- keep linear evaluation: inner-product

## Backgrounds

**1. Coefficient Embedding (SuperNeo Embedding)**
 
Given a field vector $z \in \mathbb{F}^{d \cdot n}$, split the vector into $n$ sub-vectors of length $d$ each, i.e., $z = [z_1, z_2, \ldots, z_n]$ where each $z_i := [z_{i,1}, \ldots, z_{i,d}] \in \mathbb{F}^d$. We will embed each sub-vector $z_i$ as the coefficients of a single ring element $z_i := \sum_j z_{i,j} X^{j-1} \in R_\mathbb{F}$. The resulting vector of ring elements $z := [z_1, z_2, \ldots, z_n] \in R_\mathbb{F}^n$ is the SuperNeo embedding of $z$.

$$z = \begin{bmatrix} &z_1 &|& z_2 &|& \cdots &| & z_n& \end{bmatrix}$$

<img src="/post-src/neo-n-superneo/909a2727abc103148be2d6ea48ccbe0b.jpg" style="width:50%">

**2. Linear Relation: $$\mathbb{F}$ $\to$ $\mathcal{R}$$**

**Inner Product Transform.** 

There exists a linear transform $$\bar{\cdot} : \mathbb{F}^d \to \mathbb{F}^d$$ such that for all $$a, b \in \mathbb{F}^d$$, we have the constant term

$$\text{ct}(\bar{a} \cdot b) = \langle a, b \rangle$$

where $\bar{a}$ denotes applying the transform to $a$ and embedding $\bar{a}$ into the ring.

**Example.**

Take $$\mathcal{R} = \mathbb{Z}[X]/(X^d+1)$$ as an example, such a transform is $\bar{a}(X) = a(X^{-1})$, i.e., evaluate at $X^{-1}$. In this ring, $-1 = X^d, X^{-1} = -X^{d-1}, X^{-2} = -X^{d-2}$...

> Let $a = [1,2,3,4]$, $b = [5,6,7,8] \in \mathbb{F}^4$, with $\mathcal{R} = \mathbb{Z}[X]/(X^4+1)$.

> Embed as polynomials: $a(X) = 1 + 2X + 3X^2 + 4X^3$, $\; b(X) = 5 + 6X + 7X^2 + 8X^3$.

> In this ring $X^{-1} = -X^3$,
> $$\bar{a}(X) = 1 + 2X^{-1} + 3X^{-2} + 4X^{-3} $$$$= 1 + 2(-X^3) + 3(-X^2) + 4(-X) = 1 - 4X - 3X^2 - 2X^3.$$

> The constant term of $\bar{a}(X)\cdot b(X)$ collects coefficients $\bar{a}_i \cdot b_j$ where $i+j \equiv 0 \pmod{4}$ with appropriate sign from $X^4 \equiv -1$:
> $$\text{ct}(\bar{a}\cdot b) = \bar{a}_0 b_0 - \bar{a}_1 b_3 - \bar{a}_2 b_2 - \bar{a}_3 b_1 $$
> $$= (1)(5) - (-4)(8) - (-3)(7) - (-2)(6) =  1 \times 5 + 4 \times 8 + 3 \times 7 + 2 \times 6.$$


**Linear Relations.** 

For a matrix $A \in \mathbb{F}^{m \times (d \cdot n)}$ and a witness vector $z \in \mathbb{F}^{d \cdot n}$. After embedding $z$ into ring elements $\boldsymbol{z} \in R_\mathbb{F}^n$, each row $a_i \in \mathbb{F}^{d \cdot n}$ of $A$ can similarly be split and embedded. Then the $i$-th entry of the product is:

$$[Az]_i = \langle a_i, z \rangle = \text{ct}(\bar{\boldsymbol{a}}_i \cdot \boldsymbol{z})$$

where $$\bar{\boldsymbol{a}}_i \in R_\mathbb{F}^n$$ is the transformed embedding of row $a_i$. In matrix form, defining $$\bar{\boldsymbol{A}} \in R_\mathbb{F}^{m \times n}$$ as the matrix of transformed row embeddings:

$$A z = \text{ct}(\bar{\boldsymbol{A}} \cdot \boldsymbol{z}) \in \mathbb{F}^m$$

where $\text{ct}(\cdot)$ is applied entry-wise. This means a *single ring matrix-vector multiplication* $$\bar{\boldsymbol{A}} \cdot \boldsymbol{z}$$ over $$R_\mathbb{F}^n$$ encodes all $m$ linear constraints simultaneously — the constant term extracts the result back into $$\mathbb{F}^m$$.

<img src="/post-src/salsaa/7ea41f99cf1a1fc56ba461d084072488.jpg" style="width:50%">


## Relations

**1. Structure:** is a collection of elements:
$$\mathsf{s} := \left\{ \{M_j \in \mathbb{F}^{m \times n_\mathbb{F}}\}_{j \in [t]}, f \in \mathbb{F}^{<u}[X_1, \ldots, X_t] \right\},$$
which consists of *matrices and a degree-u polynomial*.

**2. Norm-bounded CCS** 

$\mathcal{L}$: Ajtai commitment over $\mathcal{R}, (apply FFT)$.

Let $$\mathcal{L}:\mathbf{R}_{\mathbb{F}}^{n_\mathbf{R}} \to \mathbb{C}$$ be an arbitrary 
$$\mathbf{R}_\mathbb{F}$$ -module homomorphism. Let $\mathsf{s}$ be a structure. 

$$\text{CCS}(b, \mathcal{L}) = \left\{
\begin{aligned}
&\mathsf{s}; \left( c \in \mathbb{C}, x \in \mathbb{F}^{n_{\mathbb{F},\text{in}}}; w \in \mathbb{F}^{n_\mathbb{F} - n_{\mathbb{F},\text{in}}} \right) : \\
&\quad c = \mathcal{L}(z) \land \|z\|_\infty < b \land \\
&\quad f(\widetilde{M_1 z}, \ldots, \widetilde{M_t z}) \in \mathbf{ZS}_{\log m}
\end{aligned}
\right\}$$

where $z := [x, w]$, $$\mathbf{ZS}_{\log m}$$ is a 'zero set' st $f(x) = 0$ at all $\{0,1\}^{\log m}$.


**3. Norm-bounded CCS Evaluation Relation.** 

Let $\mathsf{s}$ be a structure. Let $$\mathcal{L} : \mathbf{R}_\mathbb{F}^{n_\mathbf{R}} \to \mathbb{C}$$ be an arbitrary $$\mathbf{R}_\mathbb{F}$$-module homomorphism. 
Define $$\mathcal{L}_{\text{in}} : \mathbf{R}_\mathbb{F}^{n_\mathbf{R}} \to \mathbf{R}_\mathbb{F}^{n_{\mathbf{R},\text{in}}}$$ to be the trivial $$\mathbf{R}_\mathbb{F}$$-module homomorphism that projects the first $$n_{\mathbf{R},\text{in}}$$ indices in columns. 

$$\text{CE}(b, \mathcal{L}) = 
\left\{
\begin{aligned}
&\left( \left( \mathsf{s}; \begin{pmatrix} c \in \mathbb{C}, \\ x \in \mathbb{F}^{n_{\mathbb{F},\text{in}}}, \\ r \in \mathbb{K}^{\log m}, \\ \{y_j \in \mathbf{R}_\mathbb{K}\}_{j \in [t]} \end{pmatrix} \right) ; z \in \mathbb{F}^n \right) : \\
&\quad c = \mathcal{L}(z) \land x = \mathcal{L}_{\text{in}}(z) \land \\
&\quad \|z\|_\infty < b  ~ \land \\
&\quad \forall j \in [t], {y_j} = \widetilde{M_j z(r)}
\end{aligned}
\right\}$$

## Schemes
At the beginning, CCS witnesses are decomposed into low-norm, i.e. $Norm-bounded CCS$.

Step 1. sumcheck

$\text{F}(\vec{X})$ encodes the CCS constraints (all $K$ of them). $\text{NC}(\vec{X})$ encodes the norm constraints (all $K + k$ of them). $\text{Eval}(\vec{X})$ encodes the evaluation claims (all $k$ of them) from the prior step. Finally, $Q(\vec{X})$ is defined such that if its sum over the boolean hypercube $\{0,1\}^{\log(m)}$ equals to the constructed sum $\text{T}$, then all the respective checks hold.

**Input** $\in \text{CCS}(b, \mathcal{L})^K \times \text{CE}(b, \mathcal{L})^k$

$$\left(\mathsf{s};\quad \left(c_i \in \mathbb{C},\ x_i \in \mathbb{F}^{n_{\mathbb{F},\text{in}}}\right);\ w_i \in \mathbb{F}^{n_\mathbb{F} - n_{\mathbb{F},\text{in}}}\right)_{i=1}^{K},$$

$$\left(\mathsf{s};\quad c_i \in \mathbb{C},\ x_i \in \mathbb{F}^{n_{\mathbb{F},\text{in}}},\ r \in \mathbb{K}^{\log m},\ \{y_{i,j} \in \mathbf{R}_\mathbb{K}\}_{j \in [t]};\ z_i \in \mathbb{F}^{n_\mathbb{F}}\right)_{i=K+1}^{K+k}$$

**Output** $\in \text{CE}(b, \mathcal{L})^{K+k}$

$$\left(\mathsf{s};\quad c_i \in \mathbb{C},\ x_i \in \mathbb{F}^{n_{\mathbb{F},\text{in}}},\ r' \in \mathbb{K}^{\log m},\ \{y'_{i,j} \in \mathbf{R}_\mathbb{K}\}_{j \in [t]};\ z_i \in \mathbb{F}^{n_\mathbb{F}}\right)_{i \in [K+k]}$$

![alt text](/post-src/neo-n-superneo/image-1.png)
![alt text](/post-src/neo-n-superneo/image-2.png)

Step 2. fold

![alt text](/post-src/neo-n-superneo/image-3.png)
$\mathcal{C}$ is a subtractive set over $\mathcal{R}$

Step 3. decompose

![alt text](/post-src/neo-n-superneo/image-4.png)

6 properties

- plausible post-quantum security

- pay-per-bit commitment costs: 
The cost to commit to a witness should scale with the bit-width of the witness values: for example, committing to a vector of b-bit values should be roughly b times cheaper than committing to values that span the full field. 
Group-based schemes achieve this via Pedersen or KZG commitments.
Since the Ajtai commitment scheme requires decomposing these arbitrary-norm ring elements for binding, the commitment cost is the same whether the original values are 1-bit or 64-bit.

- field-native arithmetic (the sum-check and norm checks run purely over a small field):
operate natively over a prime field (or extension field), without performing polynomial ring arithmetic.

- support for general (non-SIMD) constraint systems:
support general NP-complete constraint systems such as CCS over a single witness vector, without requiring the constraint system to be “data parallel” (SIMD, Single Instruction, Multiple Data).

- small-field (64bits, fits within a machine register) support:
work natively over small prime fields, including popular SNARK-friendly fields such as Goldilocks.

- low recursion overheads:
The recursive verifier circuit should be small enough that the per-step prover cost of IVC remains practical. More broadly, folding schemes that rely on ring sum-check techniques—such as LatticeFold and SALSAA — inherit high recursion overheads because the recursive verifier must hash ring elements rather than field elements.


--

#### Reference:

[1] Wilson Nguyen and Srinath Setty. Neo: Lattice-based folding scheme for CCS over small fields and pay-per-bit commitments. <https://eprint.iacr.org/2025/294>.

[2] Wilson Nguyen and Srinath Setty. Neo and SuperNeo: Post-quantum folding with pay-per-bit costs over small fields. <https://eprint.iacr.org/2026/242>.

---