---
layout: post
title: "The 'Sum Over 1' Problem"
excerpt_separator: <!-- more -->
tags: 
- probability
- mathematics
---

I recently came across this fun probability problem which I've dubbed the 'Sum Over 1' problem. It goes like this:

> On average, how many random draws from the interval [0, 1] are necessary to ensure that their sum exceeds 1?

To answer this question we'll use some probability theory (probability rules, distribution functions, expected value) and a dash of calculus (integration).

<!-- more -->

## Let's Begin!

Let \\(U_i\\) follow a Uniform distribution over the interval \\([0, 1]\\), where \\(i = 1, 2, \ldots\\) denotes the \\(i^{\text{th}}\\) draw. Let \\(N\\) denote the number of draws such that \\(\sum\_{i=1}^{N} U_i > 1\\) and \\(\sum\_{i=1}^{N-1} U_i \leq 1\\). Thus, \\(N\\) is a discrete random variable taking values \\(n=2,3,\ldots\\) (\\(n=1\\) is impossible because no single draw from \\([0, 1]\\) can _exceed_ 1). We seek the long-run average, or expected value, of \\(N\\) denoted \\(E(N)\\). A good first step for a numerical problem like this is to utilize computer simulations to point us in the right direction.

## Simulations with `R`

Let's generate, say, 20 random draws from \\([0,1]\\), \\((u_1, \ldots, u_{20})\\).


{% highlight r %}
draws <- runif(20)  # vector of u_1, ..., u_20
{% endhighlight %}

Consider the partial sums of these draws \\((u_1, u_1 + u_2, \sum\_{i=1}^3 u_i, \ldots, \sum\_{i=1}^{20} u_i)\\). If we index this partial sum vector from 1 up to 20, then \\(n\\) here is simply the index of the first component of this vector whose value exceeds 1. The `find_n` function accomplishes this for any vector of random draws `u`:


{% highlight r %}
find_n <- function(u) {
  which(cumsum(u) > 1)[1]  # which partial sum first exceeds 1?
}

n <- find_n(draws)
draws[1:n]
{% endhighlight %}



{% highlight text %}
## [1] 0.2858056 0.1689087 0.6259122
{% endhighlight %}

In this instance, it took 3 random draws from \\([0, 1]\\) to sum over 1. We can sample from the probability distribution of \\(N\\) by repeating this process many more times. The `sample_from_N` function takes `m` samples from the distribution of \\(N\\):


{% highlight r %}
sample_from_N <- function(m) {
  # m-by-20 matrix of random [0,1] draws
  draws <- matrix(runif(m * 20), ncol = 20)
  # for each row of 20 draws, find n
  n.many <- apply(draws, 1, find_n)
  # estimate P(N = n), n = 2, 3, ..., 10
  # (truncated b/c probs are miniscule for n > 10)
  estimated.probs <- table(factor(n.many, levels = 2:10)) / m  
  return(estimated.probs)
}

m <- 1000
approx.N <- sample_from_N(m)  # approximate prob. dist'n of N
approx.N
{% endhighlight %}



{% highlight text %}
## 
##     2     3     4     5     6     7     8     9    10 
## 0.514 0.330 0.121 0.026 0.005 0.004 0.000 0.000 0.000
{% endhighlight %}

So, we estimate \\(P(N = 2)\\) by \\(\hat{P}(N = 2) = 0.514\\), \\(P(N = 3)\\) by \\(\hat{P}(N = 3) = 0.33\\), and so forth. Of course, these are just point estimates for \\(P(N = n)\\), but we can estimate their standard errors as well by running `sample_from_N` many more times. The approximate probability distribution of \\(N\\) is depicted below.

![Probability that big N equals little n](https://lh3.googleusercontent.com/ioKESngkCW3c0mlrGbCxQqY0a18V9qbJWVF5DkSZGzUUSjDfHGEdG0o2cpfqvukrbkWTGWlrN59JwkIZxniVmEnmJGcBrCc_cib30gw9B_a1AZrwPa9Oikseo4_nUOu3Tz8mfGonPmqKNvGW36c9nyajLdsg2x-hg9UYmn0-hrrRmM75oUKW6zrKq6krthhxLFLxmOcsWLZu9xj7XLKZSYrxT1qz_izc1B5q6KKVSTbZPr7el25DI5FulWX_gFx4zvxTsgAtrm8qPYDdY26bvUbtT_l59fpOtx46ebxrfoRylpnTPPTraEMtIBeZa8v6vGkmDZLXHzlszVq-7Uhy15gw-J4KdBOkqSGaNcxMjdUvecCXtcwF5UhsdpRiYu_wOwMZP6T3-KzZW_KselaNu4N9L4Wc9eNFygmwLxs5ggLHoYuzHYFZLQdrQHtf-os1faTpwTrG87gfMBRwLUQQd9huNLRfaO_8PMGioqWd5AF1fKgkdDeARMyT8WvEwMeF_z3bDPsn1Jhkr6wbnAw8D8AbyOnpRTqWpsQ5u4HUcV_8_OH2nzsI-eBRZ84nH8ZbpNEfYU1gmk6VjfDssp16oIs0kb9gizl6QfuSm6vfuLJLBwphvwAZEBszMEvcwDrKFjCdTIpg0ZBBVqgOdmuBcTkvwg_1LedAoA=w2000-h1200-no)


The dotted line here marks the estimated expected value of \\(N\\), \\(\hat{E}(N) = \sum\_{n=2}^\infty n\hat{P}(N=n) \approx 2.71962\\). This value is tantalizingly close to the mathematical constant \\(e\approx 2.7183\\) known as _Euler's number_[^1]. Could the answer to our probability puzzle truly be \\(e\\)?

## Proof

Let's start small by finding the probability that \\(N=2\\). The probability that it takes exactly 2 random draws from \\([0, 1]\\) for their sum to exceed 1 is simply the complement of the probability that the sum of our 2 draws fall short of exceeding 1. That is, 
\\[
P(N = 2) = P(U_1 + U_2 > 1) = 1 - P(U_1 + U_2 \leq 1).
\\] 
We can interpret the probability that \\(U_1 + U_2 \leq 1\\) geometrically. Since \\(U_1\\) and \\(U_2\\) are Uniform random variables on \\([0, 1]\\), the point \\((U_1, U_2)\\) can arise anywhere in the unit square with equal probability. Thus, \\(P(U_1 + U_2 \leq 1)\\) corresponds exactly to the proportion of the unit square shaded in the following image. <br />

<center><img src='https://lh3.googleusercontent.com/HlUsKsGFMKry72ToBIn2q7inhirXgI2eoh7t3zogcfPXVqsgk_B4XH_MkykSZdtSHOFMzV10WG2mDsmimJJc6BC2OhAQ6T30Kzj83HmPDbi8gRECHuCfuwOMMLy9aogpOIypHzlI-pg_VUic46U6Z0tE6JMyhBbNBxDnHfSQEAbpzTnGF3dmq1OX36WUWKYFazHga-HEsc-6BJpUZia94332QqS_SxVZamIvEpjp8X1-0AfCu18jNF_ERmccTv_N6P1WZNDPsvrwZEdGHXfIfmxji8b4QjdnAeESsnPww3CpR97CDNBi0H7tDQrtnhgw5LsEovGjRpOfbe6jcddzDONbtP2K5qEYESBZtzELgMcmA_EPWOjCnLB2Et-0RLTYUrSCK8dfbJWYs9E-xGHwUUphrja8GweuZ_tG4zkZQ-IJe6kWxMONkRDwQcpzLuOCoi4JOrXwBJO0zGEXpNi73lQLqCP3WsxuMJL2FAQfFjr42Fy-DvCmeW5x8iONGnmekZajPtZJeTwRdSTPQu8G1bh0s4-Gykh096SgAUiqvV3EM57yZBuHStJovtHAR0QhjhnD83j0cHTUfjpNNlR8HBsHNsF-THlJWHTQNFdHs2Vj1P3CUYXGVbnpRd1LqvjO48VRxy1HWJY_J4IA3DC_anewr9WTkqQhgg=w335-h333-no' alt='2-dimensional unit simplex'/></center>

This shaded region is referred to as a _2-dimensional unit simplex_[^2] with area
\\[
P(U_1 + U_2 \leq 1) = \int_0^{1}\int_0^{1-u_2}du_1du_2 = \int_0^{1}(1-u_2)du_2 = 1/2
\\]
So \\(P(N=2) = 1 - 1/2 = 1/2\\), which agrees nicely with our simulation estimate \\(\hat{P}(N = 2) = 0.514\\). Now for the probability that \\(N=3\\), it is tempting to proceed by finding the exact probability
\\[
P(N = 3) = P(U_1 + U_2 + U_3 > 1, U_1 + U_2 \leq 1),
\\]
but dealing with both inequality constraints simultaneously is annoying.  It is helpful to recognize that \\(P(N = 3) = P(N \leq 3) - P(N = 2)\\) where
\\[
P(N \leq 3) = P(U_1 + U_2 + U_3 > 1) = 1 - P(U_1 + U_2 + U_3 \leq 1).
\\]
Similar to the previous case, the probability that the sum of three random draws from [0,1], \\(U_1, U_2,\\) and \\(U_3\\), does _not_ exceed 1 is given by the proportion of the unit cube shaded in the following image. <br />

<center><img src='https://lh3.googleusercontent.com/C4pGVVIku7pI74iwDYmA4DosnjZB2_b9DgALWc-eR2WwBGMUOqq1LPNMKbbpS9R08JVwyGJbettU6ffZImo6hdFCuYAyNUChmoBgjprk7DyIXkAodje2p3JeMRfNgUxABvjAI2uer3dCpOTfeOOTF2iYme2TuX0TOVftrxPgYdkIX4ezS-7--oYdSUWdJup6iMIZUp2p-dEh1NkGd89jKlb1vK883mC5ib9NTUQsYGG1Nzo3bJKw8IDHUJy0LF7OuMTHA4OP5hwA0TZyq9qSc2eowDnmjMN_fSic_UkmksP2uKaQzK8oneuPtBTvXMKLVb-3-1tfX1oU8KIIxPy_ei6R59xLvHYL_AIpTr0Jo0D1Il79mwOpYe0ke3lqRe_TvPSTVP7mxUBmH5LQbtpXkUtmfLQKI-JTB8Uc0nweGe3hve81TpZP183OVicWXFrwXmTAdYhWznYEjVX8qqqJlxlEfWlwSVltWFyvIN1eTqRQIy3pGj5lR89eWnx3_zkwiFptoWuSOjsi707ITBW8pk91F77OPCe0XE5-Ln3OcPtMsqblgN0JnmwYHAM0owX3apNN8ee2hfSjLdlnAMkJI81Qkz07xeE3XfAe_zBBWvYFYclLx1GPKJDrjlQ-Kl1MfKV70ICIoQRTdU_9KBalS42bwFUC5fXQpws=w393-h400-no'></center>

This shaded region is a _3-dimensional unit simplex_ with volume
\\[ \begin{align}
P(U_1 + U_2 + U_3 \leq 1) &= \int_0^{1}\int_0^{1-u_3}\int_0^{1-u_3-u_2}du_1du_2du_3 \\\
&= \int_0^{1}\int_0^{1-u_3}(1-u_3-u_2)du_2du_3 \\\
&= \int_0^{1}\frac{1}{2}\left[1 - 2u_3 + u_3^{2}\right]du_3 \\\
&= \frac{1}{2}\left[1 - 1 + \frac{1}{3}\right] \\\
&= 1/6.
\end{align}
\\]
So \\(P(N=3) = P(N\leq 3) - P(N=2) = (1 - 1/6) - 1/2 = 1/3\\) which again agrees with our simulation estimate of \\(\hat{P}(N = 3) = 0.33\\).

Foregoing a formal proof, we can generalize our results on the unit simplex: the \\(n\\)-dimensional unit simplex has "volume" given by \\(1 / n!\\). For our purposes, \\(P(N \leq n) = 1 - 1/n!\\). Thus, for any \\(n\geq 2\\), 
\\[ \begin{align}
P(N = n) &= P(N \leq n) - P(N \leq n - 1) \\\
&= \left[1 - \frac{1}{n!}\right] - \left[1 - \frac{1}{(n-1)!}\right] \\\
&= \frac{1}{(n - 1)!} - \frac{1}{n!} \\\
&= \frac{n - 1}{n!}.
\end{align}
\\] 
We've got everything we need to find the expected value of \\(N\\).
\\[ \begin{align}
E(N) &= \sum\_{n = 2}^\infty n P(N = n) \\\
&= \sum\_{n = 2}^\infty n\left(\frac{n - 1}{n!}\right) \\\
&= \sum\_{n = 2}^\infty \frac{1}{(n - 2)!} \\\
&= \sum\_{n = 0}^\infty \frac{1}{n!}
\end{align}
\\] 
Recall the exponential series is given by \\(e^{x} = \sum\_{n = 0}^{\infty} \frac{x^{n}}{n!}\\). Thus, \\(E(N)\\) is equivalently \\(e\\). Yes that's right: we expect, on average, \\(e\\) random draws from the interval \\([0, 1]\\) to ensure their sum exceeds 1!

[^1]: A convenient way to remember \\(e\\): Take 0.3 from 3 (2.7) followed by the election year of former U.S. president Andrew Jackson twice in a row (1828).  That is, \\(e \approx 2.718281828\\). Okay, maybe it's not _that_ helpful, but you're slightly more prepared for American presidential history trivia!

[^2]: An \\(n\\)-dimensional unit simplex is the region of \\(n\\)-dimensional space for which \\(x_1 + x_2 + \cdots + x_n \leq 1\\) and \\(x_i > 0, i = 1, 2, \ldots, n\\).  A unit simplex in one dimension (1D) is boring; it's just the unit interval \\([0, 1]\\). In 2D, the unit simplex takes the form of a triangle bounded by \\(x_1 + x_2 \leq 1, x_1 > 0, x_2 > 0\\). In 3D, we get a tetrahedron, in 4D a hypertetrahedron, and in \\(n\\) dimensions -- well, an \\(n\\)-dimensional simplex.
