# Rounding for Uncapacitated Facility Location

Suppose the following scenario: A company needs to supply customers in a given region that have different demands, these supplies come from different facilities, which can be bought or rented. The Uncapacitated Facility Location problem tries to solve a similar problem where

> **Problem** (Uncapacitated Facility Location)\
> Let $C$ be the set of customers, $F$ is the set of facilities, such that every customer $i \in C$ has a cost $c_{ij}$ when supplied from facility $j \in F$. However, every facility has an installation cost $f_i$. A solution to this problem is a subset $F' \subseteq F$ of installed facilities such that
> $$\min_{F' \subseteq F} \sum_{j \in F'} f_j + \sum_{i \in C} \min_{j \in F'} c_{ij}$$

This problem is NP-hard, but has approximations when the customers and facilities are in a metric space.
As we cannot measure distance between customers directly, we change the triangular inequality to allow changes between customers and facilites, in special, if $i, i' \in C$ and $j, j' \in F$, $c$ is a metric space if

$$c_{ij} \leq c_{ij'} + c_{i'j} + c_{i'j'}$$

## Linear Relaxation
The approximation we study here comes from a linear programming formulation of the problem.
Here, we define $y_j \in \{0, 1\}$ a indicator that the facility has been installed and $x_{ij} \in \{0,1\}$ another indicator that links the customer $i \in C$ to the facility $j \in F$.

$$
\begin{align}
\min_{x_{ij}, y_{j}} \quad&\sum_{j \in F} f_j y_j + \sum_{i \in C} c_{ij} x_{ij}&\nonumber\\
\mathrm{s.t.} \quad&\sum_{j\in F} x_{ij} = 1 \quad &\forall i \in C\nonumber\\
    &x_{ij} \leq y_j &\forall (i, j) \in C \times F\nonumber\\
    &0 \leq x_{ij} \leq 1 &\forall (i, j) \in C \times F\nonumber\\
    &0 \leq y_{j} \leq 1 &\forall j \in F\nonumber\\
\end{align}
$$

Let $(x^*, y^*)$ an optimal solution, probably rational, for the relaxed problem. As the customer decision can be split into many facilities, we can find a customer neighbourhood $N(i) = \{j \in F : x^*_{ij} > 0\}$. An initial heuristic is, as we only have to open a single facility for each customer, we can open only the cheapest from each neighbourhood, *i.e.*

$$F' = \{\arg\min_{j \in N(i)} f_i : i \in C\}$$

However, this heuristic can be costly due to a myoptic choice made for each customer, we could save resources by taking a further facility for a customer if this facility attends many customers at the same time.

Another strategy is to partition the set $F$ into similar facilities and select one of each subset. For example, if there is a subset of customers $J \subset C$ such that the union of their neighbourhood is the whole facility set, every other customer could be attendend by a facility that was already attending another customer $i \in J$.
We could measure the customers that share the same facilities by a second-order neighbourhood,

$$N^2(i) = \{k \in C : N(i) \cap N(k) \neq \empty\}$$