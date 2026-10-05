# Mathematical Foundations

This note summarizes the mathematical work underlying the implementation in this repository.

The focus is **Probability of Default (PD) estimation for Low Default Portfolios (LDPs)**, where the number of observed defaults is too small for naive empirical estimates to be sufficiently stable or conservative.

---

## 1. Statistical setup

Consider a portfolio with:

- $N$ exposures;
- $k$ observed defaults;
- an unknown Probability of Default $p$.

For exposure $i$, define

$$
X_i =
\begin{cases}
1, & \text{if exposure } i \text{ defaults},\\
0, & \text{otherwise}.
\end{cases}
$$

Under the homogeneous independent-default assumption,

$$
X_i \sim \mathrm{Bernoulli}(p),
$$

and therefore

$$
k = \sum_{i=1}^{N} X_i
\sim \mathrm{Binomial}(N,p).
$$

The empirical estimator is

$$
\widehat p = \frac{k}{N}.
$$

For a Low Default Portfolio, $k$ is often equal to $0$, $1$, or only a few defaults. In such cases, $\widehat p$ can be unstable and, when $k=0$, gives the non-conservative estimate $\widehat p=0$.

---

## 2. Classical Pluto–Tasche approach

The project studies a prudent PD estimator based on a high quantile of a distribution associated with the unknown default probability.

In the formulation used in the project,

$$
p \sim \mathrm{Beta}(k+1,N-k).
$$

Its density is

$$
f(p)
=
\frac{N!}{k!(N-k-1)!}
p^k(1-p)^{N-k-1}
\mathbf 1_{[0,1]}(p).
$$

Instead of using the posterior mean, a prudent estimate $p^\star$ is obtained as a high quantile:

$$
\mathbb P(p < p^\star)=q,
$$

where $q$ is typically $90\%$ or $95\%$.

Equivalently,

$$
\int_0^{p^\star} f(p)\,dp=q.
$$

The interpretation is straightforward: at confidence level $q=95\%$, the chosen PD lies in the upper $5\%$ tail of plausible PD values and is therefore deliberately conservative.

---

## 3. Closed-form case: no observed defaults

When

$$
k=0,
$$

the distribution becomes

$$
p \sim \mathrm{Beta}(1,N),
$$

with density

$$
f(p)=N(1-p)^{N-1}.
$$

The quantile equation is

$$
\int_0^{p^\star}N(1-p)^{N-1}\,dp=q.
$$

Hence

$$
1-(1-p^\star)^N=q,
$$

which gives

$$
\boxed{
p^\star=1-(1-q)^{1/N}
}.
$$

### Numerical example

For

$$
q=95\%, \qquad N=3000,
$$

we obtain

$$
p^\star
=
1-(0.05)^{1/3000}
\approx 9.98\times10^{-4}.
$$

Therefore

$$
\boxed{
p^\star \approx 0.0998\% \approx 0.10\%
}.
$$

The estimator remains strictly positive even though no default has been observed.

---

## 4. One observed default

For

$$
k=1,
$$

we obtain

$$
p \sim \mathrm{Beta}(2,N-1),
$$

with density

$$
f(p)=N(N-1)p(1-p)^{N-2}.
$$

Integrating yields

$$
\mathbb P(p<p^\star)
=
1-(1-p^\star)^N
-
Np^\star(1-p^\star)^{N-1}.
$$

Thus $p^\star$ solves

$$
\boxed{
1-(1-p^\star)^N
-
Np^\star(1-p^\star)^{N-1}
=q
}.
$$

This equation is solved numerically.

For

$$
q=95\%, \qquad N=3000,
$$

the project obtains approximately

$$
\boxed{
p^\star \approx 0.158\%
}.
$$

As expected, the prudent PD is larger than in the zero-default case.

---

## 5. General Beta prior extension

A natural extension is to introduce a proper Beta prior

$$
p \sim \mathrm{Beta}(\alpha,\beta).
$$

With the binomial likelihood,

$$
k\mid p \sim \mathrm{Binomial}(N,p),
$$

conjugacy gives

$$
\boxed{
p\mid(k,N)
\sim
\mathrm{Beta}(\alpha+k,\beta+N-k)
}.
$$

The prior mean is

$$
\mathbb E[p]
=
\frac{\alpha}{\alpha+\beta}.
$$

This makes the parameters interpretable:

- relatively small $\alpha$ and large $\beta$ imply a low prior PD;
- relatively large $\alpha$ and small $\beta$ imply a high prior PD.

The prudent estimate is again obtained through a high posterior quantile.

---

## 6. Hierarchical extension and shrinkage

Suppose the portfolio is divided into $J$ segments. For segment $j$, denote

$$
N_j = \text{number of exposures},
$$

$$
k_j = \text{number of defaults},
$$

and

$$
p_j = \text{segment PD}.
$$

The hierarchical model assumes a common portfolio-level distribution:

$$
\boxed{
p_j \sim \mathrm{Beta}(\alpha,\beta),
\qquad j=1,\ldots,J.
}
$$

After observing segment-level data,

$$
\boxed{
p_j\mid(k_j,N_j)
\sim
\mathrm{Beta}
\left(
\alpha+k_j,
\beta+N_j-k_j
\right).
}
$$

The key idea is that $\alpha$ and $\beta$ are shared across segments.

This creates **shrinkage**:

- large, information-rich segments remain strongly driven by their own observations;
- small, information-poor segments are pulled toward the portfolio-wide level;
- extreme segment PD estimates caused only by small sample sizes are reduced.

---

## 7. Why shrinkage matters

Consider the following illustrative segmentation:

| Segment | Exposures | Defaults |
|---|---:|---:|
| Industry | 3000 | 0 |
| Services | 2500 | 1 |
| Finance | 200 | 0 |

Applying the classical zero-/low-default approach independently can produce a much larger PD for the smallest segment simply because its $N$ is small.

The project illustrates this with classical prudent estimates of approximately

$$
\mathrm{PD}_{\mathrm{Industry}}\approx0.10\%,
$$

$$
\mathrm{PD}_{\mathrm{Services}}\approx0.19\%,
$$

$$
\mathrm{PD}_{\mathrm{Finance}}\approx1.47\%.
$$

The Finance segment appears more than ten times as risky as Industry despite having no additional observed default.

Under a shared hierarchical structure, the example produces much closer estimates:

$$
\mathrm{PD}_{\mathrm{Industry}}\approx0.0545\%,
$$

$$
\mathrm{PD}_{\mathrm{Services}}\approx0.0768\%,
$$

$$
\mathrm{PD}_{\mathrm{Finance}}\approx0.0804\%.
$$

The purpose is not to force all segments to have the same PD, but to regularize estimates when local information is scarce.

---

## 8. Empirical-Bayes calibration

Instead of fixing $\alpha$ and $\beta$ externally, they can be estimated from the segmented portfolio.

The project uses the marginal likelihood

$$
\boxed{
L(\alpha,\beta)
=
\prod_{j=1}^{J}
\frac{
B(\alpha+k_j,\beta+N_j-k_j)
}{
B(\alpha,\beta)
}
}
$$

where $B(\cdot,\cdot)$ is the Beta function.

Equivalently, the optimization can be performed on the log-likelihood

$$
\ell(\alpha,\beta)
=
\sum_{j=1}^{J}
\left[
\log B(\alpha+k_j,\beta+N_j-k_j)
-
\log B(\alpha,\beta)
\right].
$$

The fitted hyperparameters capture the portfolio-wide default structure and are then used in each segment posterior.

---

## 9. Wilson score approach

The empirical estimator is

$$
\widehat p=\frac{k}{N}.
$$

A classical normal approximation starts from

$$
\frac{\widehat p-p}
{\sqrt{p(1-p)/N}}
\approx
\mathcal N(0,1).
$$

The usual Wald interval replaces the unknown $p$ inside the variance by $\widehat p$. This behaves poorly for rare events and can collapse when $k=0$.

The Wilson approach instead solves the score inequality in $p$.

For a normal quantile $z$, the upper Wilson bound is

$$
\boxed{
p_{\mathrm{Wilson}}^{\mathrm{upper}}
=
\frac{
\widehat p+\frac{z^2}{2N}
+
z\sqrt{
\frac{\widehat p(1-\widehat p)}{N}
+
\frac{z^2}{4N^2}
}
}{
1+\frac{z^2}{N}
}
}.
$$

This provides a positive prudent bound even when

$$
k=0
\quad\Longrightarrow\quad
\widehat p=0.
$$

In that case,

$$
p_{\mathrm{Wilson}}^{\mathrm{upper}}
=
\frac{z^2/N}{1+z^2/N},
$$

which remains strictly positive.

---

## 10. Mathematical link to the implementation

The notebook implements these ideas numerically:

1. estimate the empirical benchmark PD;
2. repeatedly sample sparse-default portfolios;
3. compute classical Pluto–Tasche quantiles;
4. compute Wilson intervals;
5. segment the portfolio by credit quality;
6. fit shared hierarchical Beta hyperparameters;
7. compute segment-level posterior quantiles;
8. compare prudent estimates with the empirical benchmark.

The numerical experiments therefore sit on top of the mathematical structure summarized in this document rather than being purely black-box computations.

---

## 11. Main modeling trade-off

The methods can be interpreted through a prudence-versus-stability trade-off.

**Empirical PD**

$$
\text{low adjustment, high sampling sensitivity}
$$

**Classical prudent estimators**

$$
\text{strong conservatism}
$$

**Hierarchical estimator**

$$
\text{prudence + cross-segment stabilization}
$$

The central motivation of the hierarchical approach is therefore to preserve prudent PD estimation while reducing the instability created by very small segment samples.

---

## Disclaimer

This document is a concise mathematical companion to an academic project conducted in collaboration with EY Quantitative Advisory Services (QAS). The derivations, implementation and interpretations presented in this repository are the author's own academic work and should not be interpreted as official EY methodology or advice.
