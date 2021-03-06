---
layout: post
title:  "Independence and Cantor's Diagonal Argument"
date:   2018-08-28
author: "Evgeni Ovcharov"
categories: maths
tags: [Probability]
---

Let our probability space $(\Omega, \mathcal B, \lambda)$ be the unit interval $\Omega=[0,1]$ with the Borel subsets $\mathcal B$ and the Lebesgue measure $\lambda$. Is it possible to find an infinite sequence of independent and identically distributed random variables $X_1$, $X_2$, $\dots$ of any given distribution supported on this probability space?

Surprisingly, yes, and the proof relies on a version of Cantor's diagonal argument.

### Proof

It is sufficient to find a sequence of independent and uniformly distributed random variables $U_1$, $U_2$, $\dots$, on $[0,1]$. Then if $F$ is any cumulative distribution function, the sequence $X_n = F^{-1}(U_n)$ has the desired property. Thus we proceed to construct the required sequence $U_1$, $U_2$, $\dots$

In order to apply Cantor's diagonal argument, we first need to write an arbitrary point $a \in [0,1]$ as an infinite binary fraction

$$
\begin{equation*}
a = 0.\omega_1\omega_2\omega_3\dots, \quad \omega_i \in \{0,1\}.
\end{equation*}
$$

Intuitively, we need to partition the information about $a$ into an infinite collection of portions and use each portion to construct a new random variable.

We urge the reader to try finishing the proof on his or her own. Here is my approach.

### Applying Cantor’s diagonal argument

So let $\mathcal A_1$ be an infinite subsequence of $\mathcal A = \\{ \omega_1, \omega_2, \omega_3, \dots \\}$ such that the remaining subsequence of binary digits is also infinite. We select another infinite subsequence $\mathcal A_2$ of the remainder with the same condition. We may continue this process ad infinitum and obtain an infinite collection of subsequences $\mathcal A_i$. To each subsequence $\mathcal A_i$ we correspond a real number $a_i\in[0,1]$ with binary expansion given by the subsequence. We finish this construction by taking $U_i = a_i$.

Let us now show that each $U_i$ is uniform. This follows from the fact that the value of $U_i$ is a binary fraction in $[0,1]$ whose digits have been drawn independently from the set $\\{0,1\\}$. Clearly, this process produces an arbitrary number on $[0,1]$. The variables $U_i$ are also independent because they do not contain any information about each other.


### Remark

Although aesthetically satisfying, Cantor's diagonal argument brought us to a conclusion which clashes deeply with our intuition. Indeed, it allowed us to effectively clone a single random variable into an infinite sequence of independent random variables. We are reminded once more that real numbers are collections of infinite information, which is something we tend to forget when we associate finite data with real numbers. And one infinite collection contains as much information as an infinity of infinite collections. The argument would have failed on probability spaces containing only rational points for example. Nevertheless, such arguments have their use in mathematics as sometimes they demonstrate a negative result - an impossibility of a certain property. In other cases like this one, they demonstrate a positive result, which cannot be observed directly, but only in terms of finite approximations.
