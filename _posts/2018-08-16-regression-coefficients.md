---
layout: post
title:  "Unbiasedness and Consistency of the Regression Coefficients"
date:   2018-08-16
categories: maths
tags: [Regression]
---

Here we consider the basic asymptotic properties of the coefficient estimators in a simple linear regression. Many readers when first acquainted with the subject stay under the expression that this is a rather tedious and technical matter. In what follows, we would like to show that when approached from the right perspective, the subject becomes rather intuitive and clear.  

## Derivation of the estimators

We consider a simple linear regression model

$$
  Y = \beta_0 + \beta_1 X + \epsilon,
$$

where $X$ and $Y$ are random variables, $\beta_0$, $\beta_1$ are constants and $\epsilon$ is the random error. We assume that the observations $(x_1,y_1), \dots, (x_n,y_n)$ are independently and identically distributed. In the ordinary least squares (OLS) method, we want to minimize the mean squared error

\begin{equation}\label{objective}
   \min J(b_0, b_1) = \frac 1 n \sum_i (y_i - b_0 - b_1 x_i)^2
\end{equation}
in terms of the unknown constants $b_0$, $b_1$. Solving

$$
  \frac{\partial J(b_0, b_1)}{\partial b_0} = \frac 2 n \sum_i (y_i - b_0 - b_1 x_i) = 0
$$

for $b_0$, we obtain $\hat \beta_0 = \bar y - b_1 \bar x$. After finding the optimal value for $b_1$, which we denote by $\hat \beta_1$, we will get the relation $\bar y  = \hat \beta_0 + \hat \beta_1\bar x$. This means that the regression line always passes through the point $(\bar x, \bar y)$. Since the objective function $J$ is convex, its critical points are minima. That is why we may substitute $\hat \beta_0 = \bar y - b_1 \bar x$ in the expression for $J(b_0, b_1)$ in $(\ref{objective})$ and eliminate $b_0$

$$
J(b_0, b_1) = J(b_1) = \frac 1 n \sum_i ((y_i - \bar y)  - b_1 (x_i-\bar x))^2.
$$

Comparing the above with $\eqref{objective}$, we notice that centering the data leads to a regression model with zero free term, that is, to a regression line through the origin. (Of course for the original, uncentered data $\hat\beta_0$ would typically not be zero.) Solving for $b_1$ in

$$
  \frac{\partial J(b_1)}{\partial b_1} = \frac 2 n \sum_i (x_i-\bar x)((y_i - \bar y)  - b_1 (x_i-\bar x)) = 0
$$

we obtain that

$$
   \hat \beta_1 = \frac {c_{xy}}{d_x} = \frac{\frac 1 n \sum_i (x_i-\bar x)(y_i - \bar y)}{\frac 1 n \sum_i (x_i-\bar x)(x_i - \bar x)}.
$$

Notice that $\hat \beta_1$ is the ratio of the sample covariance between $X$ and $Y$ and the sample variance of $X$.

## Asymptotic properties

Here, we address the questions whether the estimators $\hat \beta_0$ and $\hat \beta_1$ are unbiased and consistent. <!--To quantify their accuracy, we are going to compute their variance as well.--> We first treat the case of the $\hat \beta_1$ estimator. The trick is to write it in the form

$$
   \hat \beta_1 = \beta_1 + \text{``an error term"}.
$$

In the expression of $c_{xy}$ we substitute $y_i$ with the model relation $y_i - \bar y = \beta_1(x_i - \bar x) + \epsilon_i$. We arrive immediately at

\begin{equation}\label{beta_1}
   \hat \beta_1 = \beta_1 + \frac{\sum_i (x_i-\bar x)(\epsilon_i - \bar \epsilon)}{\sum_i (x_i-\bar x)(x_i - \bar x)} = \beta_1 + \frac {c_{x\epsilon}}{d_x},
\end{equation}

where $c_{x\epsilon}$ denotes the sample covariance between $(x_1,\dots, x_n)$ and $(\epsilon_1,\dots,\epsilon_n)$. Notice that since $\sum_i (x_i-\bar x)\bar \epsilon = 0$ we may freely add or subtract this term to the above expression.

The standard assumptions for the error terms $\epsilon_i$ are that they are mutually independent and also $\operatorname E[\epsilon_i] = 0$, which imposes no real restriction as the mean error may always be subsumed in the free term $\beta_0$. In connection to the predictors, some models make the quite restrictive assumption of independence between the error terms and the predictors. Here we are going to assume the weaker condition $\operatorname E[\epsilon_i \mid X] = 0$ instead.  As an immediate consequence of the latter we have that

$$
   \operatorname E[\epsilon_i] = \operatorname E [\operatorname E[\epsilon_i \mid X]] = 0
$$

and also that $X$ and $\epsilon$ are uncorrelated. Indeed,

$$
  \operatorname{cov}(X,\epsilon) = \operatorname E[X\epsilon] - \operatorname E[X]\operatorname E[\epsilon] = \operatorname E \operatorname E[X\epsilon \mid X]] - 0 = 0.
$$

We may write  $\hat \beta_1$ in the form

$$
  \hat \beta_1 = \beta_1 + \sum_i \phi(x_1,\dots, x_n )\epsilon_i.
$$

Using the fact that $\operatorname E[\epsilon_i \mid x_1,\dots, x_n] = 0$, we find that $\hat \beta_1$ is unbiased

$$
   \operatorname E[\hat \beta_1] = \beta_1 + \operatorname E [\phi(x_1,\dots, x_n )\operatorname E[\epsilon_i \mid x_1,\dots, x_n]] = \beta_1.
$$

For $\hat \beta_0 = \bar y - \hat \beta_1 \bar x$, we have

$$
  \operatorname E[\hat \beta_0] = \operatorname E[\bar y] - \operatorname E [\operatorname E[\hat \beta_1 \bar x\mid x_1,\dots, x_n]] = \operatorname E[Y] - \beta_1 \operatorname E[X] = \beta_0.
$$

Thus $\hat \beta_0$ is also an unbiased estimator.

By the law of large numbers $c_{x\epsilon} \rightarrow \operatorname{cov}(X,\epsilon)=0$ and $d_x \rightarrow \operatorname D[X]$, the variance of $X$. In view of $\eqref{beta_1}$, we conclude that $\hat \beta_1 \rightarrow \beta_1$ is a consistent estimator. Similarly,

$$
  \hat \beta_0 = \bar y - \hat \beta_1 \bar x \rightarrow \operatorname E[Y] - \beta_1 \operatorname E[X] = \beta_0
$$

is also a consistent estimator.


## Numerical example

Here, we are going to verify the law of large numbers empirically. We shall check whether the sample covariance converges to the population covariance as the number of trials tends to infinity. Let us consider $Y \sim N(0, X^2)$, $X \sim U(0,1)$. We have that $\operatorname E[Y\mid X] =0$ and from the law of total expectation $\operatorname E[Y] = 0$ and $\operatorname E[XY] = 0$. Hence, $\operatorname{cov}(X,Y)= \operatorname E[XY] - \operatorname E[X]\operatorname E[Y] = 0$, but $X$ and $Y$ are not independent as the knowledge of $X$ alters the probability distribution of $Y$.

Below is a code in R which produces the numerical experiment. Here $m_{xy}$ is the sample mean of $XY$, $c_{xy}$ is the sample covariance of $X$ and $Y$, $d_x = c_{xx}$ is the sample variance of $X$, and  $d_y = c_{yy}$ is the sample variance of $Y$.

{% highlight r %}
m_xy = numeric(); c_xy = numeric(); d_x = numeric(); d_y = numeric();
size = c(10, 100, 1000, 10000, 100000)
for(n in size){
  x = runif(n); y = rnorm(n, 0, x*x);
  m = mean(x*y); m_xy = c(m_xy, m);
  c_xy = c(c_xy, m - mean(x)*mean(y))
  d_x = c(d_x, var(x)); d_y = c(d_y, var(y))
}
data = data.frame(m_xy = m_xy, c_xy = c_xy, d_x = d_x, d_y = d_y, row.names = size)
{% endhighlight %}

The results are shown in the table below.

|$n$  | $m_{xy}$      | $c_{xy}$      | $d_x$       | $d_y$       |
|--------	|-------------	|-------------	|------------	|------------	|
| 10 	|  -0.0220692 	|  -0.0130559 	|  0.0931474 	|  0.1335045 	|
| 100 	|  0.0177529 	|  0.0153785 	|  0.0878564 	|  0.1566800 	|
| 1000 	|  -0.0008616 	|  0.0005846 	|  0.0819234 	|  0.1919942 	|
| 10000 	|  -0.0047650 	|  -0.0015213 	|  0.0832527 	|  0.2027653 	|
| 100000 	|  0.0008292 	|  0.0004920 	|  0.0833878 	|  0.1992950 	|
