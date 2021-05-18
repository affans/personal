---
title: "Expected Number of Steps in a Random Walk"
date: 2021-05-02
draft: false
Description: "Hellow world"
---

The follow counter-intuitive result is one of my favourite. The expected number of steps that it takes an unbiased random walk $X_n$ on $\mathbb{Z}$ starting at 0 to get to 1 is infinite. The proof: 

Let $S = \min \\{ n > 0 \mid X_n = +1 \\} $ be a RV denoting the number of steps we take until we reach +1. Note that $S$ must be an odd number and so we have, for some $k \in \mathbb{Z}^+$,  $$P(S = 2k + 1) = C_k \frac{1}{2^{2k+1}}$$  where $C_k$ is the total number of paths of length $2k$ that begins and ends at 0, also known as the *Catalan Number*. The $n$th Catalan number is given directly in terms of binomial coefficients by 
$$ C_k = \frac{1}{1 + k}\binom{2k}{k}$$
We can now calculate the expected value of the random variable. 
$$E[S] = \sum_{k = 0}^{\infty} (2k + 1)C_k\frac{1}{2^{2k+1}}$$ 
Asymptotically, the Catalan numbers grow as 
$$C_n \sim \frac{4^n}{n^{3/2} \sqrt{\pi}}$$
therefore 
$$C_k\frac{1}{2^{2k+1}} \sim \frac{1}{2k^{3/2} \sqrt{\pi}}$$
which implies 
$$(2k + 1)C_k\frac{1}{2^{2k+1}} \sim \frac{2k + 1}{{2k^{3/2} \sqrt{\pi}}} > \frac{1}{k}$$
But since $\sum \frac{1}{k}$ diverges, we have $E[S] = \infty$. 

This result can generalize to finding the expected value for reaching $M > 1$. For example for $M = 10$, let $S_{10} = \min \\{ n > 0 \mid X_n = +10 \\}$, and so we have $$E[S_{10}] \geq E[S] = \infty$$

Pretty cool, I think.