---
layout:     post
title:      "Machine Learning Theory - Markov's Inequality and Chebyshev's Inequality"
date:       2016-04-07 12:00:00
author:     "jxzhuge12"
catalog:    true
latex:      true
---

There are 5 critical inequalities in Machine Learning Theory:

* [Markov's Inequality]({{ page.url }})
* [Chebyshev's Inequality]({{ page.url }})
* Chernoff Hoeffding Bounding
* Hoeffding's Inequality
* McDiarmid's Inequality

---

#### Markov's Inequality

> If $X$ is a nonnegative random variable and $\epsilon>0$. Then
>
> $$P[x\geq\epsilon]\leq\frac{E(X)}{\epsilon}$$

#### Chebyshev's Inequality

> Let $X$ be a random variable with finite expected value and finite non-zero variance. Then for any real number $\epsilon>0$:
>
> $$P[|X-E(X)|\geq\epsilon]\leq\frac{var(X)}{\epsilon^2}$$

#### Proof for Markov's Inequality

First of all, we define **indicator function**:

> For an event $A$, let $I(A)$ be the indicator of the event:
>
> $$y = \left\{ \begin{array}{ll}
1 & \textrm{if $A$ is true}\\
0 & \textrm{else}
\end{array} \right.$$