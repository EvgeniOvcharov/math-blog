---
layout: post
title:  "Computing an Integral"
date:   2018-08-23
categories: maths
---

### Problem
Compute the integral

\begin{equation}
  I = \int_0^\infty \frac 1 {1+x^4} dx.
\end{equation}

### Answer
***

This seems to be a popular problem illustrating various mathematical techniques. The standard approach is to use complex analysis and the [Cauchy method of residues](https://en.wikipedia.org/wiki/Residue_theorem). The problem may be solved with more elementary techniques by using a series of [clever substitutions](https://www.youtube.com/watch?v=CIRGU-Zc9h4&feature=share). Both approaches rely on quite lengthy computations to obtain the answer

$$
I =  \pi \frac {\sqrt 2} 4.
$$

Here, we would like to show that the answer may be obtained quite easily in the form of power series.

### Power series solution

We write

$$
I = \int_0^1 \frac 1 {1+x^4} dx + \int_1^\infty \frac 1 {1+x^4} dx = I_1 + I_2.
$$

For the first integral we have

$$
I_1 = \int_0^1 \sum_{k=0}^\infty (-1)^k x^{4k} dx = \sum_{k=0}^\infty (-1)^k \frac 1 {4k + 1}.
$$

For the second integral we make the substitution $x = 1/y$ and get

$$
I_2 = \int_0^1 \frac {y^2} {1+y^4} dy = \int_0^1 \sum_{k=0}^\infty (-1)^k y^{4k + 2} dy = \sum_{k=0}^\infty (-1)^k \frac 1 {4k + 3}.
$$

So the final result is

$$
I = \sum_{k=0}^\infty (-1)^k \frac 1 {4k + 1} + \sum_{k=0}^\infty (-1)^k \frac 1 {4k + 3}.
$$

For example, if we take the first few members of each series we obtain the approximation

$$
I \approx \left(1 - \frac 1 5 + \frac 1 9 - \frac 1 {13} + \frac 1 {17}\right) + \left(\frac 1 3 - \frac 1 7 + \frac 1 {11} - \frac 1 {15}\right) \approx 1.1077,
$$

whose error is less than $0.003$ of the exact value of $I$.

It would be an interesting exercise to find the sums of the above power series in closed form.
