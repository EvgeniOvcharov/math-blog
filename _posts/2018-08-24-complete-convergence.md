---
layout: post
title:  "Complete Convergence and the Borel-Cantelli Lemma"
date:   2018-08-24
categories: maths
---

Suppose we are given an infinite sequence of random variables

\begin{equation}\label{eq: X}
X_1, X_2, \dots
\end{equation}

defined on some probability space $(\Omega, \mathcal A, \operatorname P)$. For purposes of comparison we list three different modes in which the sequence may be convergent to zero.

* $(i)$ The sequence $\eqref{eq: X}$ converges to zero in probability if for every $\epsilon>0$  

$$
\begin{equation*}
  \lim_{n\rightarrow \infty} \operatorname P(|X_n|>\epsilon) = 0.
\end{equation*}
$$

* $(ii)$ The sequence $\eqref{eq: X}$ converges to zero almost surely if for every $\epsilon>0$  

$$
\begin{equation*}
  \lim_{n\rightarrow \infty} \operatorname P(\{|X_n|> \epsilon\} \cup \{|X_{n+1}|> \epsilon\} \cup \cdots) = 0.
\end{equation*}
$$  

It is easily seen that this is equivalent to the usual condition $\operatorname P(\lim_{n\rightarrow\infty} X_n = 0)=1$

* $(iii)$ The sequence $\eqref{eq: X}$ converges to zero completely if for every $\epsilon>0$  

$$
\begin{equation*}
  \lim_{n\rightarrow \infty} \operatorname P(\{|X_n|> \epsilon\}) + \operatorname P(\{|X_{n+1}|> \epsilon\}) + \cdots = 0.
\end{equation*}
$$

Clearly, $(iii)$ implies $(ii)$ and $(i)$, and $(ii)$ implies $(i)$. The example: $\Omega = [0,1]$, $\operatorname P$ is the Lebesgue measure, $X_n = 1$ for $0<\omega < 1/n$, $X_n=0$ otherwise, shows that $(ii)$ does not imply $(iii)$.

However, the second Borel-Cantelli lemma shows that if $X_n$ are independent, then $(ii)$ implies $(iii)$ and hence in that case $(ii)$ and $(iii)$ are equivalent. Why is this not a contradiction in view of the example?
