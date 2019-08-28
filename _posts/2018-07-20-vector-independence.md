---
layout: post
title:  "Are two random vectors independent if their components are pairwise independent?"
date:   2018-07-20 23:52:33 +0300
categories: maths
tags: [Probability]
---

Let $X=(X_1, X_2)$ and $Y=(Y_1, Y_2)$ be two random vectors in $\operatorname{R}^2$. Suppose that $X_i$ and  $Y_j$ are independent for each pair of indices. We have the following questions.

1. Are $X$ and $Y$ independent?
2. Are $X$ and $Y$ independent if each of them is bivariately normally distributed?
3. Are $X$ and $Y$ independent if they are jointly normally distributed?

## Answer
***

Let $X_1, X_2 \in U(0,1)$ be uniformly distributed and independent. Define the variables

$$ \begin{align*}
Y_1 &= X_1 + X_2 \qquad\operatorname{mod 1} \\
Y_2 &= X_1- X_2 \qquad\operatorname{mod 1},
\end{align*} $$

where the $\operatorname{mod 1}$ operation means that we identify all integer values on the real line and turn it into a circle of circumference one. Notice that $Y_1 \mid X_1 \sim U(0,1)$, hence the distribution of $Y_1$ does not depend on $X_1$. By the same token $Y_1$ is independent of $X_2$ as well. The argument also applies to $Y_2$, which is independent of $X_1$ and $X_2$. Hence all pairs of components are independent. However, the knowledge of $X$ fully determines the value of $Y$, hence $X$ and $Y$ are not independent.

We shall answer the second question by reducing it to the first one. To that end, let us consider the variables $X_1 \in N(0,1)$ and $X_2 \in N(0,1)$, $X_1$ and $X_2$ independent, and apply the transformation

$$ \begin{align*}
Z_1 &= F(X_1) + F(X_2) \qquad\operatorname{mod 1} \\
Z_2 &= F(X_1) - F(X_2) \qquad\operatorname{mod 1},
\end{align*} $$

where $F$ is the cumulative distribution function of the standard normal distribution. Since $F(X_i) \in U(0,1)$, it follows that $Z_1$ and $Z_2$ have a standard uniform distribution. We now define the variables $Y_1$ and $Y_2$ such that

$$ Y_1 | Z_1 \sim N(Z_1,1), \quad Y_2 | Z_2 \sim N(Z_2,1). $$

We conclude that $X_i$ and $Y_j$ are independent for all pair of indices. However, the knowledge of $X$ fully determines the value of $Z$, which is a parameter in the distribution of $Y$. Hence $X$ and $Y$ are not independent.

If $X$ and $Y$ are jointly normally distributed, then pairwise independence does imply vector independence.
