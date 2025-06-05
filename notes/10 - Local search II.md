# Local Search part II
We are now focus on local search for the minimum max-degree spanning tree problem.
This problem wants to find a spanning tree in a graph $G$ such that its maximum vertice degree is minimized. This problem is NP-complete, by reduction from Hamiltonian path (a 2-degree tree spanning tree).

$$\min_{T \in \mathcal{ST}(G)} \Delta(T)$$
where $\Delta(T) = \max_{v \in V_T} d_T(v)$ is the maximum degree of the tree.

Let $T^0$ an initial solution for the problem, a local search operates on a certain neighbourhood from $T^0$, we can define the operation over a subset $V^k \subset V_T$ such that we can reduce the degree of a vertice $v \in V^k$ by removing an edge $e = (v, \cdot) \in E_T$ and including another $e' \in E_G \setminus E_T$ such that $\Delta(T \cup \{e'\} \setminus \{e\}) < \Delta(T)$.

$$T' \in \mathcal{N}(T) \iff \exist v \in V(T),  $$




## Potential function
The idea of a potential function $\phi: \Omega \to \mathbb{R}$ that taks a solution and returns a value.
We'd like to design a function that decreases very quickly by the iterative algorithm, *i.e* the solution $x^{t+1} = \mathcal{A}(x_t)$ is reduced such that $\phi(x^{t+1}) \cdot \mathcal{O}(n^c)< \phi(x^t)$ for all $x^t$.
Therefore, 