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
* [Chernoff-Hoeffding Bound]({{ page.url }}/#chernoff-hoeffding-bound)
* [Hoeffding's Inequality]({{ page.url }}/hoeffdings-inequality)
* [McDiarmid's Inequality]({{ page.url }}/mcdiarmids-inequality)

---

## Markov's Inequality

> If $X$ is a nonnegative random variable and $\epsilon>0$. Then
>
> $$P[X\geq\epsilon]\leq\frac{E(X)}{\epsilon}$$

#### Proof for Markov's Inequality

First of all, we define **indicator function**:

> For an event $A$, let $I(A)$ be the indicator of the event:
>
> $$I(A) = \left\{ \begin{array}{ll}
1 & \textrm{if $A$ is true}\\
0 & \textrm{else}
\end{array} \right.$$

We have $X\geq\epsilon I(X\geq\epsilon)$. Taking expectation on both sides, we get

$$E(X)\geq\epsilon E(I(X\geq\epsilon))=\epsilon P(X\geq\epsilon)$$

## Chebyshev's Inequality

> Let $X$ be a random variable with finite expected value and finite non-zero variance. Then for any real number $\epsilon>0$:
>
> $$P[\vert X-E(X)\vert\geq\epsilon]\leq\frac{var(X)}{\epsilon^2}$$

#### Proof for Chebyshev's Inequality

From Markov's inequality, we define

$$Y=\vert X-E(X)\vert^2$$

Then

$$P[Y\geq \epsilon^2]\leq\frac{E(Y)}{\epsilon^2}=\frac{var(X)}{\epsilon^2}$$

---
_The previous two inequalities are easy to understand compared with the following three inequalities._

## Chernoff-Hoeffding Bound

_The following theorem is a **special case** of chernoff bound_

> Let $x_1, \cdots, x_m$ be i.i.d. Bernoulli random variables, where for every $i$:
>
> $$P(x_i=1)=p_i, P(x_i=0)=1-p_i$$
>
> Let
>
> $$\bar{X}=\frac{1}{m}\sum_{i=1}^mx_i, p=\frac{1}{m}\sum_{i=1}^mp_i$$
>
> We have
>
> $$P(\vert\bar{X}-p\vert>\epsilon)\leq 2e^{-2\epsilon^2m}$$

#### Moment-Generating Functions (MGFs)

> MGF of a random variable $X$ is defined as
>
> $$M_X(t)=E[e^{tX}], t\in\mathbb{R}$$

MGF has some important properties:

1. If $M_X(t)$ is defined for $t\in(-t^\ast,t^\ast)$ for some $t^\ast>0$, then
    * All the moments of $X$ exist, and $M_X(t)$ has derivatives of all orders at $t=0$ where $\forall k, E[X^k]=\frac{\partial^kM}{\partial t^k}\vert_{t=0}$
    * $\forall t\in(-t^\ast,t^\ast)$, $M_X(t)$ has Taylor's expansion $M_X(t)=\sum_k\frac{E[X^k]}{k!}t^k$
2. If $x$ and $y$ are independent random variables, then the $M_{x+y}(t)=M_x(t)M_y(t)$
3. For any constants $a$ and $b$, $M_{ax+b}(t)=e^{bt}M_x(at)$

#### Jensen's Inequality

> Let $X$ be a random variable, and $f$ be a convex function. Then 
>
> $$f(E[X])\leq E[f(X)]$$

#### Proof for Chernoff-Hoeffding Bound

Define $y_i=x_i-p_i$, $\bar{y}=\frac{1}{m}\sum_{i=1}^my_i$. We need to show $P(\vert\bar{y}\vert>\epsilon)\leq2e^{-2\epsilon^2m}$

Conseder each of $P(\bar{y}>\epsilon)$ and $P(\bar{y}<\epsilon)$

For $t>0$, look at 

$$P(\bar{y}>\epsilon)=P(e^{t\bar{y}}>e^{t\epsilon})\leq\frac{E[e^{t\bar{y}}]}{e^{t\epsilon}}=\frac{M_{\bar{y}}(t)}{e^{t\epsilon}}$$

> Lemma 1: For all $t>0$, $M_{y_i}(t)\leq e^{t^2/8}$
> 
> Proof: Recall  $$y_i = \left\{ \begin{array}{ll}
1-p_i & x_i=1\\
-p_i & x_i=0
\end{array} \right.$$
> 
> $$M_{y_i}(t)=E[e^{ty_i}]$$
> 
> $$=p_ie^{t(1-p_i)}+(1-p_i)e^{-tp_i}$$
> 
> $$=e^{-tp_i}(p_ie^t+(1-p_i))$$
>
> $$=exp(-tp_i+ln(p_ie^t+(1-p_i)))$$
> 
> Look at $g(t)=-tp_i+ln(p_ie^t+(1-p_i))$, for some $t^\ast\in[0,t]$
> 
> $$g(t)=g(0)+g'(0)t+g^{\prime\prime}(t^\ast)\frac{t^2}{2}$$ 
> 
> We can show $g^{\prime\prime}(t)\leq\frac{1}{4}$ $\forall t^\ast$.
> 
> Thus $g(t)\leq\frac{t^2}{8}$

Using properties of MGFs, $M_{\bar{y}}(t)=\prod_{i=1}^mM_{y_i}(\frac{t}{m})$

Using Lemma 1, $M_{\bar{y}}(t)\leq\prod_{i=1}^mexp(\frac{t^2}{8m^2})=exp(\frac{t^2}{8m})$

$$P(\bar{y}>\epsilon)\leq\frac{M_{\bar{y}}(t)}{e^{t\epsilon}}\leq\frac{exp(\frac{t^2}{8m})}{e^{t\epsilon}=exp(-(t\epsilon)-\frac{t^2}{8m})$$

Pick $t$ that maximizes the $P(\bar{y}>\epsilon)$

$$t=4m\epsilon\Rightarrow P(\bar{y}>\epsilon)\leqe^{-2m\epsilon^2}$$

The other direction is the same.

## Hoeffding's Inequality

> Let $x_1, \cdots, x_m$ be independent random variables, where for every $i$:
>
> $$a_i\leq x_i\leq b_i$$
>
> Let
>
> $$\bar{X}=\frac{1}{m}\sum_{i=1}^mx_i$$
>
> For any $\epsilon>0$, we have
>
> $$P(\vert\bar{X}-E[\bar{X}]\vert>\epsilon)\leq exp(-\frac{2\epsilon^2m^2}{\sum_i(a_i-b_i)^2})$$
>
> $$P(\vert\bar{X}-E[\bar{X}]\vert<-\epsilon)\leq exp(-\frac{2\epsilon^2m^2}{\sum_i(a_i-b_i)^2})$$

## McDiarmid's Inequality

> Let $f:\mathbb{R}^m\rightarrow\mathbb{R}$ be such that $\forall i$, and all $x_1, \cdots, x_i, \cdots, x_m, X'_i$
>
> $$\vert f(x_1, \cdots, x_i, \cdots, x_m)-f(x_1, \cdots, X'_i, \cdots, x_m)\vert\leq\Delta_i$$
>
> Let $x_1, \cdots, x_m$ be independent random variables, then
>
> $$P[f(x_1, \cdots, x_m)-E[f(x_1, \cdots, x_m)]>\epsilon]\leq exp(-\frac{2\epsilon^2}{\sum_i\Delta_i^2})$$
>
> $$P[f(x_1, \cdots, x_m)-E[f(x_1, \cdots, x_m)]<-\epsilon]\leq exp(-\frac{2\epsilon^2}{\sum_i\Delta_i^2})$$

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

<a target="_blank" href="http://zhuanlan.zhihu.com/p/19901452">
    <span class="fa-stack fa-lg">
        <i class="fa fa-circle fa-stack-2x"></i>
        <i class="fa fa-stack-1x fa-inverse">知</i>
    </span>
</a>[我只要这么多样本！ - 比生活简单多了（知乎专栏 · 作者：张熤）](http://zhuanlan.zhihu.com/p/19901452)

<a target="_blank" href="https://en.wikipedia.org/wiki/Chernoff_bound">
    <span class="fa-stack fa-lg">
        <i class="fa fa-circle fa-stack-2x"></i>
        <i class="fa fa-wikipedia-w fa-stack-1x fa-inverse"></i>
    </span>
</a>[Chernoff Bound](https://en.wikipedia.org/wiki/Chernoff_bound)

<a target="_blank" href="https://en.wikipedia.org/wiki/Moment-generating_function">
    <span class="fa-stack fa-lg">
        <i class="fa fa-circle fa-stack-2x"></i>
        <i class="fa fa-wikipedia-w fa-stack-1x fa-inverse"></i>
    </span>
</a>[Moment-Generating Function](https://en.wikipedia.org/wiki/Moment-generating_function)

<a target="_blank" href="https://en.wikipedia.org/wiki/Hoeffding%27s_inequality">
    <span class="fa-stack fa-lg">
        <i class="fa fa-circle fa-stack-2x"></i>
        <i class="fa fa-wikipedia-w fa-stack-1x fa-inverse"></i>
    </span>
</a>[Hoeffding's Inequality](https://en.wikipedia.org/wiki/Hoeffding%27s_inequality)

<a target="_blank" href="https://en.wikipedia.org/wiki/Doob_martingale#McDiarmid.27s_inequality">
    <span class="fa-stack fa-lg">
        <i class="fa fa-circle fa-stack-2x"></i>
        <i class="fa fa-wikipedia-w fa-stack-1x fa-inverse"></i>
    </span>
</a>[McDiarmid's Inequality](https://en.wikipedia.org/wiki/Doob_martingale#McDiarmid.27s_inequality)