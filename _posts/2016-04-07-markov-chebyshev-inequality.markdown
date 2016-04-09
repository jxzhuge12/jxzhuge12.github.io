---
layout:     post
title:      "Machine Learning Theory - 5 Important Inequalities"
date:       2016-04-07 12:00:00
author:     "jxzhuge12"
catalog:    true
latex:      true
tags:
    - Machine Learning
---

There are 5 important inequalities in Machine Learning Theory:

* [Markov's Inequality]({{ page.url }}/#markovs-inequality)
* [Chebyshev's Inequality]({{ page.url }}/#chebyshevs-inequality)
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

We have $X\geq\epsilon I(X\geq\epsilon)$. Taking expectation on both sides, we get

$$E(X)\geq\epsilon E(I(X\geq\epsilon))=\epsilon P(X\geq\epsilon)$$

#### Proof for Chebyshev's Inequality

From Markov's inequality, we define

> $$Y=|X-E(X)|^2$$

Then

$$P[Y\geq \epsilon^2]\leq\frac{E(Y)}{\epsilon^2}=\frac{var(X)}{\epsilon^2}$$

#### Thanks

<a target="_blank" href="http://zhihu.com/question/27821324/answer/80814695?utm_campaign=webshare&amp;utm_source=weibo&amp;utm_medium=zhihu">
    <span class="fa-stack fa-lg">
        <i class="fa fa-circle fa-stack-2x"></i>
        <i class="fa fa-stack-1x fa-inverse">知</i>
    </span>
</a>[切比雪夫不等式到底是个什么概念? - 回答作者：姚岑卓](http://zhihu.com/question/27821324/answer/80814695?utm_campaign=webshare&amp;utm_source=weibo&amp;utm_medium=zhihu)

<a target="_blank" href="https://en.wikipedia.org/wiki/Markov%27s_inequality">
    <span class="fa-stack fa-lg">
        <i class="fa fa-circle fa-stack-2x"></i>
        <i class="fa fa-wikipedia-w fa-stack-1x fa-inverse"></i>
    </span>
</a>[Markov's Inequality](https://en.wikipedia.org/wiki/Markov%27s_inequality)

<a target="_blank" href="https://en.wikipedia.org/wiki/Chebyshev%27s_inequality">
    <span class="fa-stack fa-lg">
        <i class="fa fa-circle fa-stack-2x"></i>
        <i class="fa fa-wikipedia-w fa-stack-1x fa-inverse"></i>
    </span>
</a>[Chebyshev's Inequality](https://en.wikipedia.org/wiki/Chebyshev%27s_inequality)