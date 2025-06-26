Very commonly used tool to deal with scale. 

#### **Statement**: 
Given $n$ points in a high dimensional space $\mathbf{ R}^d$ and an error parameter $\epsilon \in (0,1)$ then there exists a mapping $\mathcal{F}: \mathbf{R^d} \rightarrow \mathbf{R^k}$ where $k \approx\mathcal{O}\left( \frac{\log n}{\epsilon^2} \right)$ such that $\forall x,y$
$$(1-\epsilon) \left\lVert \mathcal{x-y}\right\rVert_{2}^2 \leq\left\lVert\mathcal{F(x) -F(y)} \right\rVert_{2}^2 \leq (1+\epsilon) \left\lVert\mathcal{x-y}\right\rVert_{2}^2$$
Advantage of going to dimension $k$ is that distance calculation in dimension $d$  is not too far off from the distance between the mapped points in the space of dimension $k$ and so we can do a cheaper computation and still get a good enough value to work instead of the true value.

So we have a nice bound.

Say $n=1e6$ and say $d=1e4$ , say for some word embedding in a DeepNN then for the downstream task has to deal with 10k features. If the downstream task only needs the distance between 2 points we can either work in this large dimension or we can, due to the above lemma, map these to a smaller $k$ dimensional space, say with $\epsilon=0.1$ giving us k to be of the order$\left( \frac{6 \cdot \log 10}{\frac{1}{100}} \right) \approx 1400$

So if you only need to work with distances you do not need to deal with $10,000$ dimensions then there is a way to map it to say,$1400$ dimensional space. After this mapping we are guaranteed that for any $x,y \in \mathbf{R^{10000}}$ , $\mathcal{F(x),F(y)} \in \mathbf{R^{1400}}$ and 
$$ 0.9 \times \left\lVert\mathcal{x -y} \right\rVert_{2}^2\leq\left\lVert\mathcal{F(x) -F(y)} \right\rVert_{2}^2 \leq 1.1 \times \left\lVert\mathcal{x -y} \right\rVert_{2}^2  $$

So we have a nice bound around the original value. Seems like a good tradeoff and hence a nice and powerful lemma. 
So if we want to do a nearest neighbor algorithm , which depends purely on distances, we can do this mapping and then do the computations on the lower dimension data points as it will be easier and faster.

How to make it work? what should the map $\mathcal{F}$ be? Why should this property hold?
Also the new dimension is purely dependent on the number of data points you have in the higher dimension and the tolerance you have and is independent of the original higher dimension. 

So the amount of data affects the dimensionality of the problem. 

---
Initially, let us consider some simple linear transforms.

So consider a matrix $R \in \mathbf{R^{k\times d}}$ so that for $u \in \mathbf{R^d}$  we have $Ru \in \mathbf{R^k}$

Now the matrix has to be of the said dimension to even qualify as a valid map and so we have just one class of possible maps in the linear space.

Now to enforce the distance preservation properties.
 Let us be a bit greedy and ask for exact preservation of length after transformation.

Can this happen?
i.e. can there be $R\in \mathbf{R^{k\times d}}$ such that $\begin{align}\left\lVert u\right\rVert^2 &= \left\lVert Ru\right\rVert^2 \\ \\   u^Tu&=u^TR^TRu \\ \end{align}$


We want this to hold for any vector. Is this even possible in general?
Say we have a 2-D vector and we rotate it, then the length is preserved still. BUT the dimensionality is not reduced so a rotation is not the full answer.
In fact what we want is to have $R^TR=I_{d}$, the d-rank Identity Matrix.

![[Pasted image 20250624190104.png]]

If this is, as above, identity then we can use this R to map any vector u from d-dimensional space to k-dimensional space and still preserve length. The case $d=k$ is trivial.


For $k<d$ , what we would have is that each of the rows are orthonormal vectors as then we would have $b_{ii}=a_{i}.a_{i}=1$ and all other entries would be zero. $a_k$ is the $k^{th}$ row of the matrix. Can this happen?

Each of $a_{i}\in \mathbf{R^k}$. So we cannot possibly have such a set of vectors as they would form a basis for the space and the dimension of a basis cannot be more than $k$.

Thus a linear transformation is not possible with a single transform.
So we have disproved the existence of an isometry. JL lemma however guarantees an approximate isometry. JL lemma also says that this need not work for any $d$-dimensional vector and we only care about our $n$ data points, and so $\approx n^2$ pairwise distances.

With these two extra relaxations maybe JL lemma can be held true. But our previous argument tells us that finding a deterministic matrix to give us an approximate isometry is going to be ***very very hard***.

We are going to use the power of randomization to come up with some matrix $R$ and use probability and argue that application of this matrix to any set of vectors is going to preserve pair-wise distances with very high probability.





