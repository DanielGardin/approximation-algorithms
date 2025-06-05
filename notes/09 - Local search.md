# Local Search
Local search is a set of algorithms that iteratively moves from a viable solution to another, with better value than the previous one. Let a combinatorial optimization problem be $\min\{f(x) : x \in \Omega\}$, a local search algorithm defines a neighbourhood function $\mathcal{N}: \Omega \to \mathcal{P}(\Omega)$ and, from an initial viable solution $x^0 \in \Omega$, iteratively improves the solution by

$$x^{t+1} = \arg\min_{x \in \mathcal{N}(x^t)} f(x).$$

We call a solution $x^*\in\Omega$ such that $f(x^*) \leq f(x) \,\forall x \in \mathcal{N}(x^*)$ to be local minimum. In general, it's not true that a local search algorithm is polynomial, since the number of steps until convergence can be not bounded by a polynomial.

## Scheduling problems
We are going to approximate a general problem in parallel scheduling, denoted as $Pm \mid \min C_{\max}$. A scheduling instance is a tuple $\langle n, p, m\rangle$, where $n$ is the number of jobs, $p$ is an array of processing times for each one of the $n$ jobs, and $m$ is the number of available machines.
A solution is an assignment function $\phi: \{1, \dots, n\} \to \{1, \dots, m\}$ such that the sum of processing times on every machine is minimized, this maximum processing time value is called makespan and is defined

$$C_{\max}(\phi) = \max_{i = 1, \dots, m} \sum_{j : \phi(j) = i} p_j.$$

This problem is equivalent to a partition problem where the objective is to balance each machine load more evenly.

Given a viable solution $\phi$, we can define a neighbourhood in the solution space that is the movement of a single task to other machine, therefore

$$\phi' \in \mathcal{N}(\phi) \iff \exists! t \in[n] : (\phi'(i) = \phi(i) \land \phi'(t) \neq \phi(t))$$

We can relate the makespan of every function in the neighbourhood because they only differ by a single task, let $t$ be this task, therefore, calling $C_i(\phi)$ the sum of the processing times assigned to the machine $i$, and denote $k=\phi(t)$ and $k'=\phi'(t)$, then

$$
\begin{align}
C_{\max}(\phi') &= \max_{i = 1, \dots, m} \sum_{j : \phi'(j) = i} p_j = \max\{C_1(\phi'), \dots, C_m(\phi')\} \nonumber\\
&= \max\{\max_{i \notin \{k, k'\}}C_i(\phi), C_k(\phi) - p_t, C_{k'}(\phi) + p_t\}\nonumber\\
\end{align}
$$

We what to design a choice of $t$ and $k, k'$ such that $C_{\max}(\phi') < C_{\max}(\phi)$. Note that if we choose $k$ such that $C_k(\phi) < C_{\max}(\phi)$, then there exists another machine that is the bottleneck, therefore, $C_{\max}(\phi')$ can only increase if $C_{k'}(\phi) + p_t \geq C_{\max}(\phi)$, or equal to $C_{\max}(\phi)$.

Therefore, to obtain a better solution we must choose to move a task from the machine with maximum processing time and move to another machine $k'$ such that $C_{k'}(\phi') < C_{k}(\phi)$, *i.e.* $C_{k'}(\phi) + p_t < C_{k}(\phi)$, in special, $t$ could be choosen such that the left hand side is minimized, therefore, choose $t : \arg\min_{j : \phi(j) = k} p_j$.

> ### **Local Search algorithm for Parallel Scheduling**
> Input:
> > $n \in \mathbb{N}$, Number of jobs\
> $p \in \mathbb{N}^n$, Array with each job's processing time\
> $m \in \mathbb{N}$, Number of machines
>
> Output:
> > $\phi: [n] \to [m]$, An assignment
> ---
> **Local-Search-Pm||**$\small\mathbf{C_{max}}(n, p, m)$:\
> Set $t=0$ and
> generate any initial solution $\phi^0$ and set $k = \arg\max_{i \in [m]} C_i(\phi^0)$ and $\ell =\arg\min_{j : \phi^0(j) = k} p_j$\
> While there is $k' \neq k$ such that $C_{k'}(\phi^t) < C_k(\phi^t) - p_\ell$, do
> > $k' \gets \arg\min_{k' \neq k} C_{k'}(\phi^t)$\
> $\phi^{t+1}(\,\cdot\,) \gets \phi^t(\,\cdot\,)$\
> $\phi^{t+1}(\ell)\, \gets k$\
> $\ell \gets \arg\min_{j : \phi^{t+1}(j) = k} p_j$\
> $t \gets t+1$
>
> Return $\phi^t$

We are going to show that this algorithm is an $2-\dfrac{1}{m}$-approximation for the scheduling problem. Suppose we ran the algorithm for a number of iterations so we end up with a local minimum solution $\phi$, therefore for all $k' \neq k, C_{k'}(\phi) \geq C_k(\phi) - p_\ell$

Suppose that $\ell$ is assigned at last in the machine $k$, therefore, its starting time is $S_\ell = C_k(\phi) - p_\ell$, and every other machine ends after $S_\ell$, by the algorithm condition.
This means that $S_\ell$ is the first time a machine ends its last job, if $\ell$ was removed, therefore

$$m\cdot S_\ell = \sum_{i =1}^m \min_{k\in[m]} \tilde{C}_k(\phi) \leq \sum_{i =1}^m C_i(\phi) \, - p_\ell = \sum_{j = 1}^n p_j \, - p_\ell$$
$$\therefore S_\ell \leq \frac{\sum_{i = 1}^n p_j \, - p_\ell}{m}$$
where $\tilde{C}$ is the completion times without the job $\ell$. Therefore, we can find a limitation for our solution's makespan:
$$
\begin{align}
C_{\max}(\phi) &\leq S_\ell + p_\ell\nonumber\\
&\leq \frac{\sum_{i = 1}^n p_j \, - p_\ell}{m} + p_\ell\nonumber\\
&\leq \frac{1}{m} \sum_{i = 1}^n p_j + \left(1 - \frac{1}{m}\right) p_\ell,
\end{align}
$$
We can bound both the terms with the optimal solution $\phi^*$, given that any processing time $p_j$ is limited by the makespan, *i.e.* $p_j \leq C_{\max}(\phi^*)$, and
$$
\begin{align}
\frac{1}{m} \sum_{i = 1}^n p_j = \frac{1}{m} \sum_{i =1}^m C_i(\phi^*) &\leq \frac{1}{m} \sum_{i =1}^m \max_{k \in [m]}C_k(\phi)\nonumber\\
&\leq \frac{1}{m} \cdot m\cdot C_{\max}(\phi^*)\nonumber\\
&\leq C_{\max}(\phi^*).\nonumber
\end{align}
$$

Substituting these bounds into Equation (1),
$$
\begin{align}
C_{\max}(\phi) &\leq \frac{1}{m} \sum_{i = 1}^n p_j + \left(1 - \frac{1}{m}\right) p_\ell \nonumber\\
&\leq C_{\max}(\phi^*) + \left(1 - \frac{1}{m}\right) C_{\max}(\phi^*)\nonumber\\
&\leq \left(2 - \frac{1}{m}\right) C_{\max}(\phi^*).\nonumber
\end{align}
$$

Now we have to prove the algorithm executes in polynomial time. In order to do that, we are going to prove that the job $\ell$, selected in the algorithm, chooses one job at most one time.
Start denoting $C_{\min}(\phi^t)$ the sum of the processing times on the machine that finishes its job first, check how this is related to $S_\ell$ in the algorithm, we are going to prove that this quantity is non-decreasing throughout the algorithm.
In a given iteration $t$, we choose a task $\ell_t$ to move from the bottleneck machine to the most free machine. Therefore, we remove the task, lowering the makespan and incresing the previous $C_{\min}(\phi^t)$, which is exactly the machine which $\ell_t$ is moved to.

Therefore, we have 2 cases, if after adding $p_{\ell_t}$ to the machine it remains being the minimal processing time machine, then $C_{\min}(\phi^{t+1}) > C_{\min}(\phi^t)$, however, if the minimum machine is changed, then it cannot be smaller then $C_{\min}(\phi^t)$, since it was the minimum in the previous iteration.
Therefore $C_{\min}(\phi^{t+1}) \geq C_{\min}(\phi^t)$. As $t$ is arbitrary, we conclude that the local search increases $C_{\min}$ monotonically (*i.e.* $C_{\min}(\phi^0) \leq C_{\min}(\phi^1) \leq \cdots \leq C_{\min}(\phi^T)$)

Suppose now that some task $\ell$ is move twice, first at iteration $k$ and at $k'$, then it is the quickest task in the longest machine, therefore, $\ell$ was moved to a machine to start earlier, therefore $S_\ell(\phi^{k+1}) <  S_\ell(\phi^k)$.
More generally, as the algorithm always reduces the makespan, it cannot start earlier therefore every iteration after $k$, its starting time cannot increase, in special, $S_\ell(\phi^{k'}) <  S_\ell(\phi^k)$.
But, in $k'$ $\ell$ was chosen again, indicating that $S_\ell(\phi^{k'}) = C_{\min}(\phi^{k'-1})$, so, the inequality above is equivalent to $C_{\min}(\phi^{k'-1}) <  C_{\min}(\phi^{k-1})$. As $k' > k$, this inequality violates the monotonicity of $C_{\min}$, leading to a contradition. Hence, our supposition that a single task can be chosen twice by the local search is false.

As each task can be selected at most once, then the algorithm runs at most once per task, *i.e* at most $n$ steps, which is polynomial with respect to the input size. $\square$

---
## Greedy approach
We can rewrite the same algorithm as a greedy algorithm. To avoid generating solutions that can be improved by local search, we have to mantain the tasks such that we cannot switch machines and improve the result.
If we countinously build the solution, a way of guaranteeing the local optimal condition is to allocate the task to the machine with lowest load.
This is a greedy algorithm named **List Scheduling**

> ### **List Scheduling**
> Input:
> > $n \in \mathbb{N}$, Number of jobs\
> $p \in \mathbb{N}^n$, Array with each job's processing time\
> $m \in \mathbb{N}$, Number of machines
>
> Output:
> > $\phi: [n] \to [m]$, An assignment
> ---
> **List-Scheduling-Pm||**$\small\mathbf{C_{max}}(n, p, m)$:\
> Set $T \gets \empty$, the list of allocated tasks\
> Initialize an empty solution $\phi$.\
> While $T \neq [n]$, do
>> Select $i \in [n] \setminus T$\
> $j \gets \arg\min_{k \in [m]} \sum_{t \in T: \phi(t) = k} p_t$\
> $\phi(i) \gets j$\
> $T \gets T \cup \{i\}$

This algorithm is also an $\left(2-\frac{1}{m}\right)$-approximation for the Parallel Scheduling problem.