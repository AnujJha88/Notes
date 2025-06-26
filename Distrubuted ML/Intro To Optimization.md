
Ever seen a flock of birds flying in perfect formation? Each bird isn't executing a predetermined plan or following the leader but instead are observing their neighbors and adjusting position in real time. This is an example of distributed optimization and co-ordination.

Similarly systems and algos can also learn , optimize and make decisions in a decentralized manner.

### Topics 
- Consensus algorithms 
- Federated Learning
- Multi Agent Systems

Imagine a bunch of robots in a warehouse coordinating tasks without a centralized controller or even a group of servers processing a large dataset parallelly and efficiently.

# Lecture 
### What is it? 

Let's start with an example:
Considered how your keyboard suggests the next word when you are typing. So say there is a NN based deployed on your phone that suggests this word.  So the learning for that needs data but we are not sharing our private data to say Google every time due to privacy concerns. So Google maybe finds out the gradients for all the different users and takes an aggregate update out of these personalized updates to apply to the global copy of the model which is then rolled out to everyone as a new update. So that's how you get better predictions.

Because there's simply too many users it cannot store all the data in one place and perform a global update directly from the data so it maybe selects a subset of users and uses their data to perform the update. So the computation of the individual gradients may happen locally on your edge devices but then the global update would need to happen at a central facility. Not fully distributed as there's distributed computation and centralized communication.

Another example:

Economic Power Dispatch
![[Pasted image 20250624193309.png]]

Set of nodes that are substations are given to us. Edges indicate connectivity between substations. 
Say Node 1 has a generator given to it and it generates $P_1$ units of power at a quadratic cost($a_{1}*P_{1}^2+b_{1}*P_{1}+c_{1}$).
Say Node 4 has a generator as well and the generation cost $a_{4}*P_{4}^2+b_{4}*P_{4}+c_{4}$.

The generators may be owned by different companies and they would not want to reveal the coefficients as these would be a proxy for how efficient and how economic their hardware is. 
So at any time say we have a total load demand $P_{load}$ and we would want $$P_{load}=P_{1}+P_{2}+P_{3}+P_{4}+P_{5}+P_{6}$$as well as minimize total cost of generation of this power. So generators should not be greedily producing their max in order to reap large benefits (at odds with our social goal).

So the objective is, from PoV of a social or centralized aggregator , we would want to minimize $$\sum_{i=1}^6 a_{i}*P_{i}^2+b_{i}P_{i}\cdot c_{i}$$

subject to $\sum_{i=1}^6 P_{i}=P_{load}$

We can also have constraints like :
$\begin{cases} P_{i}\geq P_{i,min} \forall i; \\ P_{i}\leq P_{{i,max}}  \forall i\end{cases}$

Hence the generation value are bounded. We can solve this using KKT if we know the coefficients. The main issue arises when we do not know these coefficients.


Everyone wants to solve this problem but not share their cost generation coefficients. We are allowed to exchange some info with my immediate neighbor, say 1 can exchange some info with 2 and get a sense of power generation and cost coefficients of 2 without having to estimate these.

Likewise 1 also receives some info from 2. It might be difficult to communicate info with faraway nodes.
We are not trying to solve this in centralized fashion and have to do so locally. So everyone does some local optimization and also exchanges some information with nearby nodes which helps them solve the global problem.

Since communication is also local this is an example of decentralized optimization.

It is distributed as information is distributed across different agents(each substation is an agent )and each is trying to solve the problem locally by having their own estimates of generation of others, and then through some information exchange it would try to go to the optimal solution.

### What is Federated Learning

Say you have a bunch of devices.

![[Pasted image 20250625002648.png]]

Most popular update is the gradient descent..

$$W_{k+1}=W_{k}-\eta \nabla{L(W_{k})}$$

Now the loss function for the first device is based on the data that it has, and not on any other data.

If there is a centralized server that broadcasts $W_{k}$ at each iteration and then each sub-server would compute the gradient on their own data. say $\nabla{L_{i}}$ which is relayed back to the main server.

As data points are different each server would be having different value of gradients.

So what we can do is maybe take an average of all the local updates we receive and then relay the updated weights to the sub-servers.

$$W_{k+1}=W_{k}-\frac{\eta}{n} \sum_{i=1}^n\nabla_{i}{L(W_{k})}$$

So the computation of gradients is local but there is a centralized computation protocol.

So if we scale this up to say 10000 serves then waiting for the data to be relayed back from each would be quite wasteful.

So you assume that the data distribution is more or less IID-Independent and Identically Distributed, and so you use data from only say half the people and perform the update based on that, instead of waiting for everyone's data to arrive. 

So the typical definition of federated learning has some centralized elements too but in this course we will focus more on distributed or decentralized optimization as we will also look into privacy issues as well where we would not want to share any data with a centralized entity. 

2nd example still had a singular point of failure due to a centralized update point. In the first example we still have some hope of being able to generate enough power using the other nodes 