# Mathematical Foundations of Low Default Portfolio PD Estimation

**Author:** Vincent Haïk Karakoseian  
**Project:** DDEFI — EY Quantitative Advisory Services (QAS), Financial Services  
**Original report date:** May 2026

> This document is a **full Markdown transcription and English translation of the mathematical material contained in the original LaTeX report**. The structure, examples, derivations, interpretations, advantages, limitations, and modeling arguments have been retained point by point.  
> Presentation-specific LaTeX commands (page breaks, margins, logos, headers, spacing commands, etc.) are intentionally omitted because they have no mathematical content. Standard English notation is used where appropriate (for example, `Var` for variance), while the substantive reasoning of the source is preserved.

---

# Project presentation

This third-year project was carried out in collaboration with **EY**, within the **Quantitative Advisory Services (QAS)** team of the **Financial Services (FSO)** department. The work focuses on **credit-risk modeling**, and more specifically on estimating the **Probability of Default (PD)** for portfolios known as **Low Default Portfolios (LDPs)**.

PD modeling is a central issue for financial institutions because it directly enters the calculation of expected losses, regulatory capital, and the overall management of credit risk. However, some portfolios — generally composed of very high-quality assets such as banks, sovereigns, and large international corporations — exhibit extremely low historical default rates. These portfolios are referred to as **LDPs**.

The distinctive feature of LDPs is that standard statistical methods relying on large-sample behavior become difficult to apply. Because the number of observed defaults is very small, empirical estimators may become unstable, biased, or insufficiently conservative from a regulatory perspective. Alternative approaches are therefore required, including prudent upper-bound methods, Bayesian-type techniques, and more advanced statistical-inference procedures.

The main objective of this project is therefore to **identify, study, and compare several methods specifically designed for PD modeling in Low Default Portfolios**. The analysis considers:

- the quantitative behavior of the models;
- the relevance of their assumptions;
- their advantages and limitations;
- and their usefulness in an operational banking context.

---

# I. The Pluto–Tasche model

## I.1. Clarifications on the model

The **Pluto–Tasche method (2006)** is an approach designed to estimate **Probability of Default** when the number of observed defaults is **extremely small**, typically 0, 1, or 2 defaults over several years.

It is therefore particularly suited to **Low Default Portfolios**.

The problem can be illustrated by a portfolio containing:

- **10,000 exposures**;
- but only **0 or 1 observed default over 10 years**.

### What is an exposure?

An **exposure** is the elementary unit on which the probability of default is measured. It is an individual borrower or credit contract against which the bank is exposed to a possible loss.

In other words, it is one individual element of the portfolio on which a default event may occur.

Examples include:

- **An individual loan**  
  Example: a EUR 1 million loan granted to Company X  
  $\rightarrow$ counts as one exposure.

- **A credit line / overdraft facility**  
  Example: a EUR 10 million authorized credit line to Company Y  
  $\rightarrow$ counts as one exposure.

- **A bond or debt security held by the bank**  
  Example: an Italian government bond  
  $\rightarrow$ counts as one exposure.

- **A derivative counterparty**  
  Example: a swap entered into with a corporate counterparty  
  $\rightarrow$ counts as one exposure.

If the PD is estimated empirically, one obtains:

<p align="center"><img src="equations/eq_001.png" alt="Equation 1"></p>

The report argues that values of this magnitude may be considered insufficiently prudent for regulatory purposes such as Basel III.

---

## I.2. Mathematical aspects of the Pluto–Tasche model

Pluto–Tasche provides a statistical method for obtaining a **minimum prudent and justifiable PD**.

The principle used in the project is:

> Construct a confidence-type upper bound for an extremely small proportion and use the upper bound as the prudent PD estimate.

For instance, the PD can be associated with an upper bound at a 95% confidence level.

A default indicator is modeled as:

- 0: no default;
- 1: default.

Let $X_i$ denote the default indicator of exposure $i$:

<p align="center"><img src="equations/eq_002.png" alt="Equation 2"></p>

<p align="center"><img src="equations/eq_003.png" alt="Equation 3"></p>

Each exposure is modeled as

<p align="center"><img src="equations/eq_004.png" alt="Equation 4"></p>

where $p$ is the unknown PD.

If there are $N$ exposures, the total number of defaults is

<p align="center"><img src="equations/eq_005.png" alt="Equation 5"></p>

Therefore:

<p align="center"><img src="equations/eq_006.png" alt="Equation 6"></p>

The corresponding probability mass function is

<p align="center"><img src="equations/eq_007.png" alt="Equation 7"></p>

This model describes the theoretical behavior of the number of defaults.

### Distribution used for the unknown PD

The report then considers a plausible distribution for $p$ after observing $k$ defaults among $N$ exposures:

<p align="center"><img src="equations/eq_008.png" alt="Equation 8"></p>

Its density is written as:

<p align="center"><img src="equations/eq_009.png" alt="Equation 9"></p>

Because $p$ is itself a probability, its density is naturally defined only over $[0,1]$.

For two bounds $a$ and $b$,

<p align="center"><img src="equations/eq_010.png" alt="Equation 10"></p>

### Why not use the mean?

A first idea would be to use the mean of the distribution of $p$:

<p align="center"><img src="equations/eq_011.png" alt="Equation 11"></p>

However, the report argues that this estimate is not sufficiently prudent because it may underestimate PD.

A more conservative choice is therefore to specify a confidence level, for example 90% or 95%, and to search numerically for a value $p^\star$ such that

<p align="center"><img src="equations/eq_012.png" alt="Equation 12"></p>

where $q$ denotes the chosen confidence level.

The value $p^\star$ is the **quantile at level $q$**.

Equivalently, $p^\star$ solves

<p align="center"><img src="equations/eq_013.png" alt="Equation 13"></p>

Interpretation:

- $q\%$ of the plausible PD values lie below $p^\star$;
- $(100-q)\%$ lie above $p^\star$.

For example, with $q=95\%$, the chosen PD belongs to the upper 5% of plausible PD values. It is therefore conservative while remaining compatible with the observed data.

---

### Case 1: $k=0$

When no default is observed,

<p align="center"><img src="equations/eq_014.png" alt="Equation 14"></p>

The density becomes

<p align="center"><img src="equations/eq_015.png" alt="Equation 15"></p>

Hence

<p align="center"><img src="equations/eq_016.png" alt="Equation 16"></p>

<p align="center"><img src="equations/eq_017.png" alt="Equation 17"></p>

<p align="center"><img src="equations/eq_018.png" alt="Equation 18"></p>

<p align="center"><img src="equations/eq_019.png" alt="Equation 19"></p>

<p align="center"><img src="equations/eq_020.png" alt="Equation 20"></p>

Imposing the confidence level gives

<p align="center"><img src="equations/eq_021.png" alt="Equation 21"></p>

Therefore:

<p align="center"><img src="equations/eq_022.png" alt="Equation 22"></p>

<p align="center"><img src="equations/eq_023.png" alt="Equation 23"></p>

<p align="center"><img src="equations/eq_024.png" alt="Equation 24"></p>

<p align="center"><img src="equations/eq_025.png" alt="Equation 25"></p>

So the closed-form estimator is

<p align="center"><img src="equations/eq_026.png" alt="Equation 26"></p>

For $q=95\%$ and $N=3000$:

<p align="center"><img src="equations/eq_027.png" alt="Equation 27"></p>

<p align="center"><img src="equations/eq_028.png" alt="Equation 28"></p>

<p align="center"><img src="equations/eq_029.png" alt="Equation 29"></p>

<p align="center"><img src="equations/eq_030.png" alt="Equation 30"></p>

<p align="center"><img src="equations/eq_031.png" alt="Equation 31"></p>

The resulting PD is therefore approximately **0.1%**. It is very low, which is consistent with the fact that no default was observed, but it remains strictly positive.

This is the simplest case.

---

### Case 2: $k=1$

With one observed default,

<p align="center"><img src="equations/eq_032.png" alt="Equation 32"></p>

The density is

<p align="center"><img src="equations/eq_033.png" alt="Equation 33"></p>

The cumulative probability is

<p align="center"><img src="equations/eq_034.png" alt="Equation 34"></p>

The report performs the integration as follows:

<p align="center"><img src="equations/eq_035.png" alt="Equation 35"></p>

Then:

<p align="center"><img src="equations/eq_036.png" alt="Equation 36"></p>

<p align="center"><img src="equations/eq_037.png" alt="Equation 37"></p>

<p align="center"><img src="equations/eq_038.png" alt="Equation 38"></p>

Therefore $p^\star$ is obtained by solving

<p align="center"><img src="equations/eq_039.png" alt="Equation 39"></p>

Unlike the $k=0$ case, the solution is obtained numerically.

For $q=95\%$ and $N=3000$, the report obtains by numerical bisection:

<p align="center"><img src="equations/eq_040.png" alt="Equation 40"></p>

This PD is slightly higher than in the zero-default case, which is consistent with $k=1\gt0$.

---

## I.3. Advantages and disadvantages of Pluto–Tasche

### Advantages

According to the report, Pluto–Tasche is attractive for LDP PD modeling because:

- it is **prudent**, since it produces a PD greater than zero even when no historical default has been observed;
- it works even when $k$ is extremely small, such as $0$ or $1$;
- it is **simple and explainable**, rather than a black-box approach;
- it is described as **robust to small data variations**, in the sense that a small change in $N$ does not cause the PD estimate to explode;
- it does **not require heavy calibration**; a simple Python script or even a calculator can be sufficient.

### Disadvantages

The report also identifies several limitations:

- its conservative nature may lead to an **overestimation of PD**, which can penalize a bank in terms of regulatory capital;
- the choice of the confidence level can be **arbitrary**;
- it is a **univariate method**, using only $k$ and $N$ and ignoring additional information such as borrower financial characteristics, macroeconomic variables, or market volatility;
- it relies on strong assumptions of **independence and homogeneity of exposures**: each exposure is assumed to have the same PD and defaults are assumed independent, whereas real portfolios can contain heterogeneous exposures, non-negligible default correlation, and geographic or sectoral clusters.

---

# II. Possible extensions

## II.1. Changing the prior distribution of $p$

In a Bayesian framework, the Probability of Default $p$ is treated as a random variable rather than as a fixed number. It therefore has a distribution before the current data are observed.

This pre-data distribution is the **prior distribution** of $p$.

Concretely, it represents the initial knowledge or belief about the PD before observing defaults in the dataset.

The Pluto–Tasche formulation used above gives, after observing $N$ exposures and $k$ defaults:

<p align="center"><img src="equations/eq_041.png" alt="Equation 41"></p>

This is interpreted in the report as a posterior distribution for $p$.

More generally, in a Beta–Binomial Bayesian framework:

<p align="center"><img src="equations/eq_042.png" alt="Equation 42"></p>

and if the prior distribution is

<p align="center"><img src="equations/eq_043.png" alt="Equation 43"></p>

then the posterior distribution is

<p align="center"><img src="equations/eq_044.png" alt="Equation 44"></p>

The report compares:

<p align="center"><img src="equations/eq_045.png" alt="Equation 45"></p>

with

<p align="center"><img src="equations/eq_046.png" alt="Equation 46"></p>

From this comparison, it identifies the formal parameter correspondence

<p align="center"><img src="equations/eq_047.png" alt="Equation 47"></p>

That would correspond to a $\mathrm{Beta}(1,0)$ prior, which is not a proper Beta distribution because valid Beta parameters must satisfy

<p align="center"><img src="equations/eq_048.png" alt="Equation 48"></p>

The report therefore states that, as a first approximation and because $N$ is often very large, the Pluto–Tasche setup can be approximated by a Bayesian framework with a **uniform $\mathrm{Beta}(1,1)$ prior**.

Thus:

<p align="center"><img src="equations/eq_049.png" alt="Equation 49"></p>

The corresponding density is

<p align="center"><img src="equations/eq_050.png" alt="Equation 50"></p>

The report interprets this as saying that, before observing the data, every PD value between 0 and 100% is considered possible, so no informative prior knowledge is introduced.

### Non-uniform priors

An extension consists in choosing a non-uniform prior

<p align="center"><img src="equations/eq_051.png" alt="Equation 51"></p>

The report uses the usual pseudo-count interpretation:

- $\alpha-1$ is interpreted as a number of **fictitious defaults** before observing the current data;
- $\beta-1$ is interpreted as a number of **fictitious non-defaults**.

Therefore:

- with $\mathrm{Beta}(1,1)$, the prior contains 0 fictitious defaults and 0 fictitious non-defaults;
- with $\mathrm{Beta}(1,1000)$, the prior contains 0 fictitious defaults and 999 fictitious non-defaults, corresponding to a belief in a very low PD;
- with $\mathrm{Beta}(1000,1)$, the prior contains 999 fictitious defaults and 0 fictitious non-defaults, corresponding to a belief in a very high PD.

The interpretation can also be seen through the Beta mean.

If

<p align="center"><img src="equations/eq_052.png" alt="Equation 52"></p>

then

<p align="center"><img src="equations/eq_053.png" alt="Equation 53"></p>

Therefore:

- if $\alpha$ is small and $\beta$ is large,

<p align="center"><img src="equations/eq_054.png" alt="Equation 54"></p>

so a low PD is expected;

- if $\alpha$ is large and $\beta$ is small,

<p align="center"><img src="equations/eq_055.png" alt="Equation 55"></p>

so a high PD is expected;

- if $\alpha=\beta=1$,

<p align="center"><img src="equations/eq_056.png" alt="Equation 56"></p>

### Important modeling note retained from the report

The original report explicitly emphasizes that the **Pluto–Tasche method itself is not presented as a Bayesian method**. For the purpose of the extension developed in the project, the report chooses to approximate it with a Bayesian $\mathrm{Beta}(1,1)$ prior framework.

Within that generalized Bayesian framework, one can choose

<p align="center"><img src="equations/eq_057.png" alt="Equation 57"></p>

according to prior information about the portfolio.

After observing $N$ exposures and $k$ defaults, the posterior becomes

<p align="center"><img src="equations/eq_058.png" alt="Equation 58"></p>

A high posterior quantile $p^\star$ is then selected, as before.

The posterior density used in the report is written as

<p align="center"><img src="equations/eq_059.png" alt="Equation 59"></p>

The prudent quantile is determined by

<p align="center"><img src="equations/eq_060.png" alt="Equation 60"></p>

<p align="center"><img src="equations/eq_061.png" alt="Equation 61"></p>

<p align="center"><img src="equations/eq_062.png" alt="Equation 62"></p>

The report considers levels such as $q=95\%$ or $q=90\%$.

Examples of possible prior beliefs mentioned in the report include:

- bonds issued by large international corporations or large banks may have a PD around **0.05%**;
- AAA-rated government bonds may have a PD around **0.01%**;
- regional banks may have a historical PD around **0.1%**.

Such prior information may come from:

- macroeconomic views;
- Moody's ratings or studies;
- other models;
- or, more generally, any source containing relevant information about PD before observing the current dataset.

This provides a first generalization of the base Pluto–Tasche framework.

In a classical Bayesian framework, $\alpha$ and $\beta$ are fixed before observing the current data. The report then considers an alternative **Empirical Bayes** approach in which $\alpha$ and $\beta$ can instead be estimated from data.

---

## II.2. A more general hierarchical approach

Before calculating PD, it can be useful to segment the portfolio by, for example:

- **sector**: industry, services, finance, etc.;
- **rating**: AAA, AA+, AA−, BBB, etc., for sovereign or corporate bonds;
- **company size**, for corporate-bond portfolios.

Each segment $j$ then has:

- its own number of exposures $N_j$;
- its own number of defaults $k_j$;
- its own PD $p_j$.

The key idea is that the segment PDs are not treated as unrelated quantities. They share a common portfolio-level prior parameterized by $\alpha$ and $\beta$:

<p align="center"><img src="equations/eq_063.png" alt="Equation 63"></p>

The report refers to this common higher-level distribution as an **hyper-prior**.

For each segment, after observing $(k_j,N_j)$:

<p align="center"><img src="equations/eq_064.png" alt="Equation 64"></p>

The report writes the corresponding density in the form

<p align="center"><img src="equations/eq_065.png" alt="Equation 65"></p>

As before, high quantiles are then used to obtain segment-level prudent PD estimates.

### Information sharing and shrinkage

A central observation is that **data-poor segments benefit from information contained in data-rich segments**.

A common mistake would be to let $\alpha$ and $\beta$ depend independently on $j$. The report argues that this would remove the key advantage of the hierarchical approach.

Instead, a **common prior distribution** is shared by all segments and reflects the overall level of portfolio risk.

The PD of each segment is therefore a compromise between:

- information specific to that segment;
- and information from the overall portfolio.

This is the **shrinkage** mechanism:

> Small or poorly informed segments are pulled toward the global portfolio mean.

---

### Illustrative example

Consider the following segmentation:

| Segment | Number of exposures | Observed defaults |
|---|---:|---:|
| Industry | 3000 | 0 |
| Services | 2500 | 1 |
| Finance | 200 | 0 |

Using the initial Pluto–Tasche approach at a 95% level, the report obtains:

For **Industry**:

<p align="center"><img src="equations/eq_066.png" alt="Equation 66"></p>

For **Services**:

<p align="center"><img src="equations/eq_067.png" alt="Equation 67"></p>

For **Finance**:

<p align="center"><img src="equations/eq_068.png" alt="Equation 68"></p>

This result is surprising: the Finance segment is estimated to be more than ten times riskier than Industry despite having no additional observed default. The difference is driven mainly by the much smaller segment size.

The report highlights this as a known issue:

> Small segments may generate extreme PD estimates — either very high or very low.

The hierarchical model is introduced specifically to mitigate this effect.

---

### First rough estimate of the hyperparameters

In the illustrative example there is 1 observed default among 5700 exposures.

The report first proposes a rough approximation:

- $\alpha = 1 + \text{number of defaults} = 2$;
- $\beta = 1 + \text{number of non-defaults} = 5700$.

The common prior is therefore approximated as

<p align="center"><img src="equations/eq_069.png" alt="Equation 69"></p>

The segment posterior distributions are then written as:

For **Industry**:

<p align="center"><img src="equations/eq_070.png" alt="Equation 70"></p>

<p align="center"><img src="equations/eq_071.png" alt="Equation 71"></p>

<p align="center"><img src="equations/eq_072.png" alt="Equation 72"></p>

For **Services**:

<p align="center"><img src="equations/eq_073.png" alt="Equation 73"></p>

<p align="center"><img src="equations/eq_074.png" alt="Equation 74"></p>

<p align="center"><img src="equations/eq_075.png" alt="Equation 75"></p>

For **Finance**:

<p align="center"><img src="equations/eq_076.png" alt="Equation 76"></p>

<p align="center"><img src="equations/eq_077.png" alt="Equation 77"></p>

<p align="center"><img src="equations/eq_078.png" alt="Equation 78"></p>

The 95% prudent PD estimates given in the report are:

<p align="center"><img src="equations/eq_079.png" alt="Equation 79"></p>

<p align="center"><img src="equations/eq_080.png" alt="Equation 80"></p>

<p align="center"><img src="equations/eq_081.png" alt="Equation 81"></p>

These values are much closer to each other and are presented as economically more coherent than the independent segment estimates.

---

### Empirical-Bayes calibration

The report stresses that the previous values of $\alpha$ and $\beta$ are only a rough approximation.

In the actual hierarchical model, $\alpha$ and $\beta$ are estimated from data by **maximizing the marginal likelihood**.

In an Empirical-Bayes framework:

- $\alpha$ and $\beta$ are treated as unknown hyperparameters;
- they are estimated from the observed segment data.

The marginal likelihood used in the report is

<p align="center"><img src="equations/eq_082.png" alt="Equation 82"></p>

The Beta function is written as

<p align="center"><img src="equations/eq_083.png" alt="Equation 83"></p>

Therefore, optimization algorithms can search for the values of $\alpha$ and $\beta$ that maximize

<p align="center"><img src="equations/eq_084.png" alt="Equation 84"></p>

This is another level of generalization beyond the base Pluto–Tasche approach.

---

# III. Comparison with the Wilson method

## III.1. Motivation

The starting point is again a portfolio with:

- $N$ exposures;
- $k$ observed defaults.

The empirical PD estimator is

<p align="center"><img src="equations/eq_085.png" alt="Equation 85"></p>

As before:

<p align="center"><img src="equations/eq_086.png" alt="Equation 86"></p>

with

<p align="center"><img src="equations/eq_087.png" alt="Equation 87"></p>

The report uses:

<p align="center"><img src="equations/eq_088.png" alt="Equation 88"></p>

and

<p align="center"><img src="equations/eq_089.png" alt="Equation 89"></p>

For large $N$, the Central Limit Theorem motivates

<p align="center"><img src="equations/eq_090.png" alt="Equation 90"></p>

Equivalently:

<p align="center"><img src="equations/eq_091.png" alt="Equation 91"></p>

Dividing numerator and denominator appropriately by $N$ gives

<p align="center"><img src="equations/eq_092.png" alt="Equation 92"></p>

The usual normal/Wald approximation then replaces the unknown $p$ in the denominator by $\hat p$:

<p align="center"><img src="equations/eq_093.png" alt="Equation 93"></p>

For a standard normal variable $Z$,

<p align="center"><img src="equations/eq_094.png" alt="Equation 94"></p>

Therefore:

<p align="center"><img src="equations/eq_095.png" alt="Equation 95"></p>

Isolating $p$ gives

<p align="center"><img src="equations/eq_096.png" alt="Equation 96"></p>

Hence:

<p align="center"><img src="equations/eq_097.png" alt="Equation 97"></p>

Or:

<p align="center"><img src="equations/eq_098.png" alt="Equation 98"></p>

This is the classical normal interval.

---

### Problems with the classical normal interval

The report identifies several problems.

#### 1. Degeneracy when $k=0$

If $k=0$,

<p align="center"><img src="equations/eq_099.png" alt="Equation 99"></p>

and the normal interval may collapse to a zero PD estimate. The report considers this unsuitable for LDP prudential modeling because a strictly zero PD is not sufficiently conservative.

#### 2. Replacing the true variance term by an estimated one

The report writes the standardized variable as

<p align="center"><img src="equations/eq_100.png" alt="Equation 100"></p>

It recalls that

<p align="center"><img src="equations/eq_101.png" alt="Equation 101"></p>

<p align="center"><img src="equations/eq_102.png" alt="Equation 102"></p>

<p align="center"><img src="equations/eq_103.png" alt="Equation 103"></p>

<p align="center"><img src="equations/eq_104.png" alt="Equation 104"></p>

<p align="center"><img src="equations/eq_105.png" alt="Equation 105"></p>

For the variance:

<p align="center"><img src="equations/eq_106.png" alt="Equation 106"></p>

<p align="center"><img src="equations/eq_107.png" alt="Equation 107"></p>

<p align="center"><img src="equations/eq_108.png" alt="Equation 108"></p>

<p align="center"><img src="equations/eq_109.png" alt="Equation 109"></p>

The associated standard deviation is

<p align="center"><img src="equations/eq_110.png" alt="Equation 110"></p>

The report's discussion is that, for convenience, the unknown $p$ is replaced by the noisy estimator $\hat p$ in this variance term.

Thus the true standard deviation is replaced by an estimated/noisy standard deviation.

When $N$ is small, $\hat p$ can vary strongly from one sample to another.

The report illustrates this idea by noting that different samples may produce values such as:

<p align="center"><img src="equations/eq_111.png" alt="Equation 111"></p>

The point of the example is that $\hat p$ can move sharply when the effective amount of information is low.

---

### Jensen-inequality argument developed in the report

The function

<p align="center"><img src="equations/eq_112.png" alt="Equation 112"></p>

is concave because

<p align="center"><img src="equations/eq_113.png" alt="Equation 113"></p>

For a concave function, Jensen's inequality gives

<p align="center"><img src="equations/eq_114.png" alt="Equation 114"></p>

Taking $X=\hat p$:

<p align="center"><img src="equations/eq_115.png" alt="Equation 115"></p>

The report uses this to motivate a **downward bias in the plug-in variance term** $\hat p(1-\hat p)$ relative to $p(1-p)$.

It further argues that the effect becomes more material when the estimator $\hat p$ itself is highly variable.

The consequence described in the report is an overly narrow interval for $p$.

The desired interval has the form

<p align="center"><img src="equations/eq_116.png" alt="Equation 116"></p>

Equivalently:

<p align="center"><img src="equations/eq_117.png" alt="Equation 117"></p>

The report gives an illustrative comparison.

Instead of an interval such as:

- $\hat p_{\mathrm{upper}}=0.5\%$;
- $\hat p_{\mathrm{lower}}=0.05\%$;

one might obtain an interval such as:

- $\hat p_{\mathrm{upper}}=0.3\%$;
- $\hat p_{\mathrm{lower}}=0.1\%$.

The latter is narrower and therefore less prudent.

The report explains coverage as follows:

> A 95% confidence interval means that if the sampling experiment were repeated many times, approximately 95% of the intervals constructed by the procedure would contain the true PD.

For example, if the true PD were 1% and one repeated the experiment 10,000 times on independent samples, approximately 9,500 intervals should contain $0.01$ and about 500 may miss it.

If the interval is systematically too narrow, there can be more than 5% misses, so the **actual coverage** falls below 95%.

This motivates the use of the **Wilson method**.

---

## III.2. Mathematical derivation of the Wilson method

The derivation starts again from

<p align="center"><img src="equations/eq_118.png" alt="Equation 118"></p>

The goal is to derive an interval by solving directly for $p$ rather than relying on the same plug-in structure as the Wald interval.

The admissible values of $p$ are characterized through the score inequality.

The derivation in the report proceeds to the quadratic inequality:

<p align="center"><img src="equations/eq_119.png" alt="Equation 119"></p>

Squaring both sides:

<p align="center"><img src="equations/eq_120.png" alt="Equation 120"></p>

Multiplying by $N$:

<p align="center"><img src="equations/eq_121.png" alt="Equation 121"></p>

Expanding:

<p align="center"><img src="equations/eq_122.png" alt="Equation 122"></p>

Moving all terms to the left gives

<p align="center"><img src="equations/eq_123.png" alt="Equation 123"></p>

Therefore:

<p align="center"><img src="equations/eq_124.png" alt="Equation 124"></p>

This is a quadratic inequality in $p$.

Define the polynomial

<p align="center"><img src="equations/eq_125.png" alt="Equation 125"></p>

The admissible PD values lie between the two roots.

After algebraic simplification, the Wilson interval can be written in its standard form:

<p align="center"><img src="equations/eq_126.png" alt="Equation 126"></p>

In a prudent setting, the project retains the upper root:

<p align="center"><img src="equations/eq_127.png" alt="Equation 127"></p>

### Behavior when $k=0$

When

<p align="center"><img src="equations/eq_128.png" alt="Equation 128"></p>

we also have

<p align="center"><img src="equations/eq_129.png" alt="Equation 129"></p>

Then the upper Wilson bound becomes

<p align="center"><img src="equations/eq_130.png" alt="Equation 130"></p>

<p align="center"><img src="equations/eq_131.png" alt="Equation 131"></p>

Therefore:

<p align="center"><img src="equations/eq_132.png" alt="Equation 132"></p>

Even with no observed default, Wilson produces a strictly positive prudent bound.

---

## III.3. Advantages and disadvantages of the Wilson method

The Wilson method is presented as an improvement over the classical normal interval for estimating a proportion $p$ from $k$ successes over $N$ Bernoulli trials — here, $k$ defaults among $N$ exposures.

It starts from

<p align="center"><img src="equations/eq_133.png" alt="Equation 133"></p>

and solves the inequality in $p$ rather than substituting $\hat p$ for $p$ inside the variance at the critical step.

The interval is bounded by roots

<p align="center"><img src="equations/eq_134.png" alt="Equation 134"></p>

and for prudential purposes the upper root is retained:

<p align="center"><img src="equations/eq_135.png" alt="Equation 135"></p>

Here $z$ is the standard-normal quantile associated with the selected confidence level.

---

### Advantages

#### 1. Improvement over the classical normal/Wald interval

Unlike the normal interval

<p align="center"><img src="equations/eq_136.png" alt="Equation 136"></p>

the Wilson method is not based on the same direct plug-in interval construction.

The report argues that this reduces the coverage problems associated with unstable $\hat p$ values and provides a generally more reliable confidence interval.

#### 2. Non-degenerate behavior for $k=0$

Even when no default is observed and $\hat p=0$, the upper Wilson bound remains strictly positive:

<p align="center"><img src="equations/eq_137.png" alt="Equation 137"></p>

This is particularly useful in LDP prudential applications, where a zero PD is considered inappropriate.

#### 3. Operational simplicity

The formula is closed-form.

There is no need for a numerical quantile search of the type used in the Beta-quantile approach.

It is therefore:

- fast to implement;
- straightforward to audit;
- and computationally lightweight.

The report presents Wilson as an attractive alternative when a robust and simple frequentist method is desired.

#### 4. Bounds remain within $[0,1]$

The Wilson construction gives bounds that remain coherent as proportions.

This avoids some artifacts of the normal approximation, such as negative lower bounds or upper bounds larger than 1 in extreme cases.

---

### Disadvantages

#### 1. Asymptotic foundation and limitations of the normal approximation in LDPs

Wilson remains based on a normal approximation motivated by the Central Limit Theorem.

Recall:

<p align="center"><img src="equations/eq_138.png" alt="Equation 138"></p>

For large $N$,

<p align="center"><img src="equations/eq_139.png" alt="Equation 139"></p>

Equivalently:

<p align="center"><img src="equations/eq_140.png" alt="Equation 140"></p>

The report stresses that reliability is not merely a matter of "$N$ being large."

A good normal approximation requires a regime in which both

<p align="center"><img src="equations/eq_141.png" alt="Equation 141"></p>

and

<p align="center"><img src="equations/eq_142.png" alt="Equation 142"></p>

Practical rules of thumb are often written as

<p align="center"><img src="equations/eq_143.png" alt="Equation 143"></p>

Indeed, for

<p align="center"><img src="equations/eq_144.png" alt="Equation 144"></p>

we have

<p align="center"><img src="equations/eq_145.png" alt="Equation 145"></p>

and

<p align="center"><img src="equations/eq_146.png" alt="Equation 146"></p>

The Central Limit Theorem gives

<p align="center"><img src="equations/eq_147.png" alt="Equation 147"></p>

but the approximation is meaningful when the variance

<p align="center"><img src="equations/eq_148.png" alt="Equation 148"></p>

becomes sufficiently large.

Therefore one needs, in practice,

<p align="center"><img src="equations/eq_149.png" alt="Equation 149"></p>

These conditions ensure that there are sufficiently many expected defaults and non-defaults for the binomial distribution to become smooth and approximately symmetric.

By contrast, in an LDP, $Np$ may remain very small even when $N$ itself is large.

The report gives the example:

<p align="center"><img src="equations/eq_150.png" alt="Equation 150"></p>

Although $N$ is large, the expected number of defaults is only 1.

The distribution of $k$ is then highly concentrated on small integer values and is close to a Poisson distribution with parameter

<p align="center"><img src="equations/eq_151.png" alt="Equation 151"></p>

which is far from a symmetric bell-shaped distribution.

The report therefore notes that a symmetric normal approximation with support on the whole real line may:

- allocate artificial probability mass to negative values even though $k\ge0$;
- underestimate the right tail when $p$ is very small because of asymmetry;
- produce intervals whose actual probability of containing the true PD is not exactly equal to the nominal confidence level, such as 95%.

---

#### 2. No direct probability distribution for $p$

Wilson produces a **frequentist confidence interval**, not a posterior probability distribution for $p$.

Therefore one should not interpret a 95% confidence interval as:

> "There is a 95% probability that $p$ belongs to this particular interval."

The frequentist interpretation is instead:

> If the sampling experiment were repeated many times and a Wilson interval were reconstructed each time, approximately 95% of those intervals would contain the true value of $p$.

In a prudential setting, where one reasons about a single actual portfolio rather than a hypothetical infinite repetition of experiments, the report considers this interpretation less intuitive than a Bayesian posterior approach such as a Beta-quantile model.

Because Wilson does not assign a probability distribution to $p$ in the Bayesian sense, the report also notes that it does not naturally provide a hierarchical shrinkage mechanism between segments.

---

#### 3. Purely univariate method

Like classical Pluto–Tasche, Wilson depends only on:

- $N$;
- $k$;
- the selected confidence level.

It does not directly incorporate additional information such as:

- counterparty quality;
- macroeconomic variables;
- within-portfolio heterogeneity;
- an explicit segment structure;
- dependence between defaults.

It is therefore simple and robust, but structurally limited.

---

#### 4. No information-pooling mechanism between segments

When Wilson is applied segment by segment, each segment is treated independently.

There is no mechanism analogous to hierarchical shrinkage for sharing information across segments.

Consequently, for small or poorly populated segments, the upper bound may become highly conservative simply because local information is scarce.

The report notes that this is precisely what is observed empirically for the weakest segments in the numerical study.

---

#### 5. Potentially excessive prudence in practice

Even though Wilson improves on the classical normal interval, its upper bound can become highly conservative in rare-default settings, especially when:

- $N$ is moderate;
- $k$ is very small;
- or individual segments are small.

The report highlights this as an empirical finding of the project:

> In the numerical study, the Wilson upper bound is the most conservative estimator among the compared methods.

---

# Summary of the mathematical progression

The mathematical development of the project follows the sequence below.

### 1. Base LDP problem

<p align="center"><img src="equations/eq_152.png" alt="Equation 152"></p>

Empirical PD is unstable when only a few defaults are observed.

### 2. Classical prudent Pluto–Tasche-type estimate

<p align="center"><img src="equations/eq_153.png" alt="Equation 153"></p>

and a high quantile $p^\star$ is retained.

### 3. Generalized Beta prior

<p align="center"><img src="equations/eq_154.png" alt="Equation 154"></p>

leading to

<p align="center"><img src="equations/eq_155.png" alt="Equation 155"></p>

### 4. Hierarchical model

<p align="center"><img src="equations/eq_156.png" alt="Equation 156"></p>

with

<p align="center"><img src="equations/eq_157.png" alt="Equation 157"></p>

Shared hyperparameters produce **shrinkage**.

### 5. Empirical-Bayes calibration

<p align="center"><img src="equations/eq_158.png" alt="Equation 158"></p>

The hyperparameters are estimated from the data.

### 6. Frequentist benchmark: Wilson

<p align="center"><img src="equations/eq_159.png" alt="Equation 159"></p>

Wilson provides a closed-form prudent upper bound but no hierarchical information-sharing mechanism.

---

# Disclaimer

This document reproduces and translates the mathematical development of an academic project carried out in collaboration with EY Quantitative Advisory Services (QAS). The work, derivations, implementation, and interpretations presented here are the author's academic work and should not be interpreted as official EY methodology, research, or advice.
