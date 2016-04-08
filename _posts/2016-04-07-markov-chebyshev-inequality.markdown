---
layout:     post
title:      "Machine Learning Theory - Markov's Inequality and Chebyshev's Inequality"
date:       2016-04-07 12:00:00
author:     "jxzhuge12"
catalog:    true
latex:      true
---

在机器学习理论中，有5个非常重要的不等式：

* [Markov's Inequality]({{ page.url }})
* [Chebyshev's Inequality]({{ page.url }})
* Chernoff Hoeffding Bounding
* Hoeffding's Inequality
* McDiarmid's Inequality

---

#### Chebyshev's Inequality

> If $X$ is a nonnegative random variable and $\epsilon>0$. Then
$$P[x\geq\epsilon]\leq\frac{E(X)}{\epsilon}$$