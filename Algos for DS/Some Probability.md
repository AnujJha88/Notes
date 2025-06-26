## Concentration Inequalities
There is some random experiment that is conducted so there is a random variable that captures the outcome of it. We have some info on this random variable but not all. So maybe you know the average or the variance etc. but not the entire distribution.

e.g. where you do have entire distribution is a die throw. Here the sample space is $\{1,2,3,4,5,6\}$, the event space is $2^S$ the power set and the probability space is $\left\{ \frac{1}{6},\frac{1}{6},\frac{1}{6},\frac{1}{6},\frac{1}{6},\frac{1}{6}\right\}$ so we have all the information about the problem.

In a lot of real life cases we have much lesser info, maybe only the expected value but not the entire distribution. How do we still make meaningful statements about certain events? This is where concentration inequalities enable us to make statements on the values that the random variable takes if we only have partial information. 

### 1. Markov Inequality

For any non-negative random variable X and any $a>0$ 
$$P(x\geq a) \leq \frac{E[X]}{a}$$

Nice result but also quite simple.
We cannot calculate the exact value of this probability as we lack the distribution or very convoluted, so we have to make do with the mean.

So we do have a (possibly loose) upper bound.

This inequality also holds when we do have access to the distribution.

The trivial bound is $\leq 1$  but Markov gives us a slightly better bound

Actual answer depends on the distribution of the random variable and Markov gives us an upper bound for any random variable that has expected value $E[X]$. So this is quite general too. 


Also this helps us bound the chances of the variable taking on "large values".

e.g.  Let X be the distance of a data point to cluster center in K-Means Clustering algorithm.
Say we have $E[X]=5$ .
Now say we have a randomized algorithm which picks a random point and makes use of the value of our random variable X for that point.
We want to be sure that the picked point has distance not too large to the cluster center.
Say we ask: For a randomly picked point what is the chance that distance to the cluster center is $\geq 20$ 

If we only have the average distance then we can use Markov Inequality. So instead of reiterating over all the points again and counting the required points we simply apply the inequality 
	$$P[X\geq20] \leq \frac{5}{20}=\frac{1}{4}$$
Thus we cannot have more than a quarter of the points be more than 20 distance away from cluster center. So we have a rough idea of the answer.

### 2. Chebyshev
There can be many random variables with the same mean but different variances. 

If you have this extra information about the variances ( or about the random process generating these points)  then we can have a better bound.

So we have a random variable $X$ with finite expectation $E[X]$ and finite variance $\sigma^2$  then for any $k>0$

$$P(|X-E[X]|>k\cdot\sigma )\leq \frac{1}{k^2} $$

So what is the chance that the results concentrate closer to the mean. 
![[Pasted image 20250623233147.png]]

So we account for being away in both directions using absolute difference.

So how many standard deviations out can you be and how likely is it to happen.

Thus the larger the deviation the more unlikely it is going to be. And we are talking upper bounds here so it can always be worse.

### 3. Hoeffding
Let $X_{1},X_{2},\dots,X_{n}$ be independent random variables where each $X_{i} \in[a,b]$
 $$\bar{X}= \frac{1}{n}\sum{X_{i}}$$
 $$P(\bar{X}-E[\bar{X}]\geq t) \leq e^\frac{{-2nt^2}}{(b-a)^2}$$
 So we can see how well our tolerances behave.  Thus for larger $t$ the decrease is exponential. So rare events are smaller very fast. More coins also means that we can be more confident of the sample and actual mean being closer.
So for the same tolerance more runs or more random variables would make the bound stronger. 

### Other Inequalities

1. Union Bound:
   $P(A\cup B)=P(A)+P(B)-P(A\cap B)$
sometimes it might be hard to calculate the last term. So what we can do is ignore it and make this an inequality
   $P(A\cup B)\leq P(A)+P(B)$
   This also extends to $n$ events. The upper bounds may be loose.
2.  If $A \implies B$ then $A \subseteq B$ and hence $P(A)\leq P(B)$

   