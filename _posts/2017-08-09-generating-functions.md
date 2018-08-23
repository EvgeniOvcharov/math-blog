---
layout: post
title:  "Solving Recurrence Relations with Generating Functions"
date:   2017-08-09
categories: maths
---

*``A generating function is a device somewhat similar to a bag. Instead of carrying many little objects detachedly, which could be embarrassing, we put them all in a bag, and then we have only one object to carry, the bag." -- George Polya, Mathematics and plausible reasoning (1954)*

### Introduction

Often in mathematics we have to deal with recurrence relations. One of the best known examples of recurrence relations are the *Fibonacci numbers* given by the relation

$$
{\displaystyle F_{n}=F_{n-1}+F_{n-2}},
$$

with initial conditions ${\displaystyle F_{0}=1}, {\displaystyle F_{1}=1.}$ The Fibonacci sequence appeared in 1202 in the book *Liber Abaci* by the Italian mathematician Leonardo of Pisa, also known today as Fibonacci, where he tries to model the population growth of an idealized pair of newborn rabbits. The author supposes that in the beginning of each new month the number of rabbit pairs will be equal to their number $F_{n-1}$ a month ago plus the newborn pairs of rabbits, which would be equal to the number of rabbit pairs that were born two or more months ago, or $F_{n-2}$.

We present below an interesting and nontrivial mathematical problem about recurrence relations. The problem illustrates the rich mathematical theory of the subject and in particular the application of generating functions to study the solutions of recurrence relations.

### Problem

Consider a random number sequence $S_n$ generated by the relation

$$
\begin{equation}
S_{n+1} = S_n + X, \quad S_0 = 0, \quad n = 0,1, \dots,
\end{equation}
$$

where $X$ is a random variable taking with equal probability any value from a given set of natural numbers $\mathcal X = \\{n_1, n_2, ..., n_m\\}$. Let $p_k$ be the probability that the sequence $S_n$ will contain a given number $k$. Determine $p_k$ asymptotically in the form $p_k = \pi + o(1)$, where $\pi = \pi(\mathcal X)$ should be given explicitly.
<!--
show that $p_N$ is in the form $p_N = p + o(1)$ for some $p\in[0,1]$ and some $o(1)$ as $N\rightarrow\infty$, and find $p$.
-->

### Motivational example

In the simplest case where $\mathcal X = \\{1,2\\}$, $p_n$ may be found explicitly to be

$$
   p_n = \frac 2 3 + \frac {(-1)^n }{2^{n} 3},
$$

for $n\geq 1$, see below. Indeed, for $n=1$ the formula gives $p_1 = 1/2$, which is correct as we may reach $n=1$ starting from $n=0$ only if $X=1$. For $n=2$ the formula gives $p_2 = 3/4$. This is also correct as all possible pairs of values of $X$ are: $(1,1)$, $(1,2)$, $(2,1)$, and $(2,2)$. Of these, only the pair $(1,2)$ misses $n=2$ starting from $n=0$. Hence $n=2$ can be reached in 3 out of 4 ways, or $p_2=3/4$. We may continue computing $p_3$, $p_4$, ... in this way, however, at some point we should realize that there is a recurrence relation between the probabilities $p_n$, $p_{n-1}$, $p_{n-2}$ given by

$$
p_n = \frac{p_{n-1} + p_{n-2}} 2.
$$

Indeed, the sequence may reach the number $n$ only if it undergoes the following two transitions with regard to its previous state: either from $n-1$ to $n$ or from $n-2$ to $n$. The first transition happens with probability $p_{n-1}/2$ and the second transition happens with probability $p_{n-2}/2$.

Before we begin with the solution, we need to revise some basic facts.

### Linear recurrence relations with constant coefficients

A *linear recurrence relation* is an equation of the form

$$
\begin{equation}\label{eq: general k}
    x_n = c_1 x_{n-1} + c_2 x_{n-2} + \cdots + c_k x_{n-k}
\end{equation}
$$

that defines the $n$-th term in a number sequence $x_n$ in terms of the $k$ previous terms in the sequence. The coefficients $c_i$ are all assumed to be constants. If $c_k\not= 0$, the relation is said to be of order $k$. The recurrence relation here is called *linear* because all the terms in the sequence appear as first-order polynomials. This relation is also called *homogenous* because there is no constant term appearing in the equation. For example, the Fibonacci sequence is a linear homogeneous recurrence relation with constant coefficients of order 2. We briefly recall the main methods for solving linear recurrence relations with constant coefficients.

### Characteristic polynomial

The simplest recurrence relation is the first-order relation $x_n = r x_{n-1}$, $x_0=a$. Its solution is immediately seen to be $x_n=ar^n$. Looking for solutions of the general $k$-th order relation $\eqref{eq: general k}$ in the same form, leads us to consider the roots of the *characteristic polynomial*

$$
\begin{equation}\label{eq: char}
    r^n - c_1 r^{n-1} - c_2 r^{n-2} - \cdots - c_k r^{n-k}
\end{equation}
$$

of the recurrence relation $\eqref{eq: general k}$. The distinct roots $r_i$ of that polynomial induce solutions of the recurrence relation in the form $x_n = a_i r_i^n$, where $a_i$ are some constants. If $r$ is a repeated root of multiplicity $l$, then the solutions corresponding to that root have the form

$$
   x_n = a_0 r^n + a_1 n r^n + \cdots a_{l-1} n^{l-1} r^n,
$$

for some constants $a_0$, $\dots$, $a_{l-1}$. Since any polynomial of degree $k$ has $k$ roots counted with multiplicity, the roots of the characteristic polynomial always give us the full set of $k$ linearly independent solutions. The unspecified constants appearing in the general form of the solutions may be determined from the values of the first $k$ members of the recurrence relation.

### Generating functions

Sometimes finding the roots of the characteristic polynomial explicitly may not be feasible, especially if the polynomial is of degree higher than 2. Suppose, for example, that we want to show that a sequence $x_n$ generated by $\eqref{eq: general k}$ converges to some limit $L$ as $n\rightarrow \infty$, which we want to find explicitly. Such a limit would exist in the first place only if all the roots of $\eqref{eq: char}$ are in absolute value less than one, possibly with the exception of one root of multiplicity one which may be equal to one. If this is the case, $x_n$ would be of the form $x_n = L + o(1)$, where by $o(1)$ we have denoted terms which tend to zero as $n\rightarrow\infty$. Determining $L$ from the initial conditions, however, might require obtaining the general form of the solution, which may be difficult or impossible without approximation. In this case, the method of *generating functions* is indispensable.

The *generating function* of a number sequence $a_n$ is a function of the form

$$
 G(a_n;x)=\sum_{n=0}^\infty a_nx^n.
$$

If $a_n=p_n$ is the probability mass function of a discrete random variable $X$ taking values in the non-negative integers $\{0,1, ...\}$, then its generating function is called the *probability generating function* of $X$. Using the probability generating function of $X$, we may compute the expectation of $X$ in terms of the formula

$$
\operatorname E X = \sum_{n=0}^\infty n p_n = \lim_{x\rightarrow 1^{-}}\sum_{n=0}^\infty n p_nx^n = G'(p_n;1^-).
$$

The existence of the limit, either finite or infinite, is guaranteed by Abel's theorem. Generating functions are intimately related to other concepts from probability theory such as the moment generating function and the characteristic function of a random variable, but we shall not pursue this matter any further here.

Generating functions were first introduced by Abraham de Moivre in 1730 to study recurrence relations like $\eqref{eq: general k}$. Generating functions were extensively applied by Euler to study combinatorial and number theory problems in the 1750's. The name generating function was coined by Laplace in connection to what is now known as the Laplace transform and the moment generating function of a random variable.

### Examples of generating functions for simple sequences

Polynomials are generating functions to finite sequences.

The constant sequence 1,1,1,$\dots$, is associated with the key generating function,

$$
  \sum_{n=0}^\infty x^n = \frac 1 {1-x},
$$

which is the sum of the geometric series with common ratio $x$.

The substitution $x \rightarrow ax$ gives the generating function of the geometric sequence $1, a, a^2, a^3, ...$, for any constant $a$,

$$
{\displaystyle \sum _{n=0}^{\infty }(ax)^{n}={\frac {1}{1-ax}}.}
$$

By squaring the initial generating function, or by finding the derivative of both sides with respect to $x$ and making a change of running variable $n \rightarrow n+1$, we get the generating function,

$$
{\displaystyle \sum _{n=0}^{\infty }(n+1)x^{n}={\frac {1}{(1-x)^{2}}},}
$$

of the sequence $a_n = n+1$. More generally, for any non-negative integer $k$ and non-zero real value $a$, it is true that

$$
{\displaystyle \sum _{n=0}^{\infty }a^{n}{\binom {n+k}{k}}x^{n}={\frac {1}{(1-ax)^{k+1}}}\,.}
$$

Note that, since

$$
{\displaystyle 2{\binom {n+2}{2}}-3{\binom {n+1}{1}}+{\binom {n}{0}}=2{\frac {(n+1)(n+2)}{2}}-3(n+1)+1=n^{2},}
$$

we may find the generating function for the sequence $a_n=n^2$ of square numbers as a sum of more elementary generating functions

$$
{\displaystyle {\begin{aligned}G(n^{2};x)&=\sum _{n=0}^{\infty }n^{2}x^{n}={\frac {2}{(1-x)^{3}}}-{\frac {3}{(1-x)^{2}}}+{\frac {1}{1-x}}={\frac {x(x+1)}{(1-x)^{3}}}.\end{aligned}}}
$$

### Basic properties of generating functions

The generating function of a number sequence can be expressed as a rational function (the ratio of two finite-degree polynomials) if and only if the sequence is generated by a linear recurrence relation with constant coefficients.

For the next property of generating functions let us recall the *Cauchy product* of two infinite sequences. If $a_i$ and $b_i$ are two infinite number sequences, the Cauchy product of the associated power series is defined as follows:

$$
{\displaystyle \left(\sum _{i=0}^{\infty }a_{i}x^i\right)\cdot \left(\sum _{j=0}^{\infty }b_{j}x^j\right)=\sum _{k=0}^{\infty }c_{k}x^k} \quad     \text{where} \quad     {\displaystyle c_{k}=\sum _{l=0}^{k}a_{l}b_{k-l}}.
$$

The sequence $c_i$ is the called the Cauchy product of the sequences $a_i$ and $b_i$, or also the *discrete convolution* of $a_i$ and $b_i$. Thus, the product of two generating functions is the generating function of the Cauchy product of the associated sequences.

For example, the sequence of cumulative sums ${\displaystyle (a_{0},a_{0}+a_{1},a_{0}+a_{1}+a_{2},\cdots )}$ of a sequence with generating function $G(a_n; x)$ has the generating function

$$
{\displaystyle G(a_{n};x){\frac {1}{1-x}}}.
$$

Shifting sequence indices by $m$ steps to the right or $m$ steps to the left yields the following identities:

$$
{\displaystyle {\begin{aligned}
                                                x^{m}G(a_n; x)&=\sum _{n\geq m}a_{n-m}x^{n}=G(a_{n-m}; x)\\
{\frac {G(a_n;x)-a_{0}-a_{1}x-\cdots -a_{m-1}x^{m-1}}{x^{m}}}&=\sum _{n\geq 0}a_{n+m}x^{n}=G(a_{n+m}; x),
\end{aligned}}}
$$

where $a_{-1}, \dots, a_{-m}$ are all set to zero.

Differentiation and integration of generating functions yields the following identities:

$$
{\displaystyle {\begin{aligned}
G^{\prime }(a_n;x)             &=\sum _{n\geq 0}(n+1) a_{n+1}x^{n}\\
xG^{\prime }(a_n;x)      &=\sum _{n\geq 0}n a_{n}x^{n}\\
\int _{0}^{x}G(a_n;t)dt        &=\sum _{n\geq 1}{\frac {a_{n-1}}{n}}x^{n}.
\end{aligned}}}
$$

### Solution of the problem

First, without loss of generality we may assume that the numbers $n_1$, $\dots$, $n_m$ are relative prime. Indeed, if $d \geq 1$ is their greatest common divisor then $S_n$ will traverse only the numbers $\{d, 2d, 3d, \dots\}$ as $S_n$ and $X$ will be divisible by $d$. Then we may write $X=dX'$, where $X'$ is a new random variable taking values in the set $\mathcal X' = \{n_1/d, n_2/d, ..., n_m/d\}$. Then writing $S_n=dS_n'$, it is clearly enough to consider the problem only for the sequence $S_n'$.

Suppose that the elements of the set $\mathcal X$ are given in ascending order: $n_1 \leq n_2 \leq \cdots \leq n_m$. If $p_n$ is the probability the sequence $S_n$ to contain $n$ for any $n\geq0$, then we have the recurrence relation

$$
\begin{equation}\label{eq: p_n}
   p_{n} = \frac {p_{n-n_1} + p_{n-n_2}  + \cdots + p_{n-n_m}} m,
\end{equation}
$$

for $n\geq1$. Here, $p_0=1$ is the starting point, and we use the notation $p_n=0$, for $n<0$. Let $g(x) = p_0 + p_1 x + p_2 x^2 + \cdots$ be the generating function of the sequence $p_n$. Then we have the identity

$$
   m (g(x) - 1) = x^{n_1}g(x) + x^{n_{2}}g(x) + \cdots + x^{n_m}g(x).
$$

Indeed, this follows immediately from $\eqref{eq: p_n}$ for all $p_n$ with $n \geq 1$, while for $n=0$ we keep in mind that we have set $p_0=1$. Solving for $g$, we obtain

$$
\begin{equation}\label{eq: g}
   g(x) = \frac {m} {m -  (x^{n_{1}} + x^{n_{2}} \cdots + x^{n_m})} = \frac 1 {1-s(x)},
\end{equation}
$$

where $s(x)=(x^{n_{1}} + x^{n_{2}} \cdots + x^{n_m}) / m$ is the probability generating function of the random variable $X$.

We continue by noticing that $g(x)$ has a unique pole at $x=1$. If we can show that

$$
   g(x) = \frac {C} {1 - x} + f(x),
$$

where $C$ is a suitable constant and $f(x)$ is analytic around $x=1$, then it would follow that $p_n = C + o(1)$ as $n\rightarrow\infty$. Indeed, the Taylor coefficients of analytic functions around $x=1$ must tend to zero as $n\rightarrow\infty$.

Expanding the function $1-s(x)$ in a Taylor series around $x=1$, we obtain

$$
  1 - s(x) = -\mu(x-1) - \mu (x-1)^2 r(x),
$$

where $\mu = (n_1+n_2+\cdots+n_m)/m = \operatorname E X$ and $r(x)$ is some analytic function around $x=1$. It follows that

$$
   g(x) = \frac 1 {\mu} \frac 1 {1 - x}\frac 1 {1-(1-x)r(x)} = \frac 1 {\mu} \frac 1 {1 - x}\left(1+(1-x)r(x) + (1-x)^2r^2(x)+\cdots\right).
$$

Thus, $g(x)$ has the desired form with $C=1/\mu$ and $f(x) = (r(x) + (1-x)r^2(x) + \cdots)/\mu$. Hence, we conclude that

$$
   p_n = \frac 1 {\operatorname E X} + o(1)\quad \text{as} \quad n\rightarrow\infty.
$$

### Exercise 1

To illustrate the method with the characteristic polynomial, let us consider the above problem in the particular case of $\mathcal X=\\{1,2\\}$. Now the characteristic equation is given by

$$
     2x^2 = x + 1.
$$

Its roots are $1$ and $-1/2$. We see that $p_n$ is in the form $\displaystyle{p_n = 1^n a  + (-1)^n{2^{-n}}b}$, where $a$ and $b$ are some constants to be determined. We have

$$
\begin{align}
  p_0 &= a + b= 1\\
  p_1 &= a - b/2= 1/2.
\end{align}
$$

From here $a=2/3$, $b=1/3$. Thus, we obtain

$$
   p_n = \frac 2 3 + \frac {(-1)^n }{2^{n} 3},
$$

showing that $p_n\rightarrow 2/3$ as $n\rightarrow\infty$.

### Exercise 2

We again consider the above problem in the particular case of $\mathcal X=\\{1,2\\}$, but this time in terms of the generating function. In view of $\eqref{eq: g}$, we now have $g(x) = {2} / (2-x-x^2)$. The roots of the polynomial $2-x-x^2$ are $1$ and $-2$. We write

$$
  g(x) = \frac {1} {(1-x)(1+\frac x 2)} = (1+x+x^2+\cdots)(1-\frac x 2 + \frac {x^2} 4 - \cdots).
$$

Hence,

$$
p_n = 1 - \frac 1 2 + \cdots + (-1)^n \frac 1 {2^n} = \frac 2 3(1 - (-1)^{n+1}\frac 1 {2^{n+1}})  = \frac 2 3 + \frac {(-1)^n }{2^{n} 3}.
$$