# Rounding and Approximation
Rounding can be useful for designing approximation algorithms, especially when the NP-difficulty is dropped when our instance representation changes.
For example, take the Knapsack problem:

> ### **Problem** (Binary Knapsack problem)
> Let I be a set of $n$ items and 2 $n$-sized arrays $v$ and $w$, where $v_i \in \mathbb{N}$ is the value of carrying item $i$, and $s_i \in \mathbb{N}$ is its weight and a value $W \in \mathbb{N}$.
The objective is to select a subset $S \subseteq I$ such that the weight carryied by the knapsack does not exceed $W$, *i.e.* $\sum_{i \in S} w_i \leq W$ so the total value, $\sum_{i \in S} v_i$ is maximized.
>$$
\begin{align*}
\max_{S \subseteq I} \quad &\sum_{i \in S} v_i\\
\text{subject to} \quad &\sum_{i \in S} w_i \leq W
\end{align*}
$$

There are two ways of encoding an instance of this problem, as each natural number has different bases.
For example, we could store all values, weights and capacity in base 10.
Suppose $B$ is the number of digits in base 10 enough to encode every natural number in the instace, making $B$ logithmical in respect to $v_i$, $w_i$ and $W$.
The number of bits required to encode the instance is $\Theta(nB)$.

Since $W \in \mathcal{O}(10^B)$, an algorithm within this encoding that explicitly depends on $W$, would then be exponential with respect to the encode complexity. In fact, the Knapsack problem is NP-hard with this encoding.
However, the same algorithm can be polynomail with the respect of another encoding: the **unary** representation.

As the NP-hardness was introduced by representing $W$ in a logarithm encoding, we could choose a linear encoding instead.
This encoding takes exactly $W$ bits to represent the natural number $W$.
The preivous algorithm now depends linearaly on the encoding size, making the algorithm polynomial.

> #### **Definition** (Strongly NP-hard)
> A problem is strongly NP-hard if it remains NP-hard even if the instance is represented in unary.

An immediate result is that the Knapsack problem is not strongly NP-hard. This means that algorithms for solving not strongly NP-hard problems can be efficient for small values, but become impractical when the number envolved in the instance scales up. A common method for solving such problems in pseudo-polynomial time is **Dynamic Programming**\dots

## Dynamic Programming
Dynamic programming is a set of algorithms, and an algorithmic strategy that solves sequential decision problems



## Polynomial Approximation Schemes
Approximation schemes are algorithm families that can bound the approximation ratio by an arbitrary value, by the cost of a more complex algorithm. A problem is called