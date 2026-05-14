(sec-dual-fitting-setcover)=

# Dual Fitting for Greedy Set Cover

We now show how to use dual-fitting to show that a natural greedy
algorithm for weighted set cover achieves a $H_n$-approximation where
$H_n = \sum_{i=1}^n 1/i$ is the $n$ th Harmonic number. Since
$H_n = \Theta(\log n)$, we get that greedy is a $O(\log n)$
approximation.

Recall that in Weighted Set Cover, we have a universe $U$ of $n$
elements, a collection of sets $S_1, \ldots, S_m \subseteq U$ where set
$S_i$ costs $w_i$. The goal is to find a collection of sets that cover
$U$ with minimum total cost.

The LP and its dual are:

$$\begin{align*}
\text{minimize} \quad & \sum_i w_ix_i\\
\text{subject to} \quad & \sum_{i: S_i \ni e} x_i \geq 1 \quad  && \forall e \in U\\
& x \geq 0
\end{align*}$$

$$\begin{align*}
\text{maximize} \quad & \sum_e y_e\\
\text{subject to} \quad & \sum_{e \in S_i} y_e \leq w_i \quad  && \forall i \in [m]\\
& y \geq 0
\end{align*}$$

The greedy algorithm is as follows. The set $I$ is the set of indices of
sets that are chosen by the algorithm. The set $C$ is set of elements
that are covered by $I$. Intuitively, the algorithm picks the set with
the most bang-for-the-buck, i.e. cheapest per new element covered.

::: {prf:algorithm label=alg-greedy-setcover}

- Initialize $C = I = \emptyset$
- **while** $C \neq U$ **do**
  - Choose $i$ that minimizes $$\frac{w_i}{|S_i \setminus C|}$$
  - Add $i$ to $I$
- **return** $I$

:::

We construct the following dual $y$. In each iteration, if the set $S_i$
was chosen, then for each element $e \in S_i \setminus C$ (these are the
newly covered elements), set $y_e= \frac{w_i}{|S_i \setminus C|}$.

We now show that $y$ violates the dual constraints by at most a factor
of $H_n$.

::: {prf:lemma label=lem-greedy-setcover}

For every $i \in [m]$, we have
$\sum_{e \in S_i} y_e \leq H_n \cdot w_i$.

:::

::: {prf:proof enumerated=false}

Fix set $S_i$ and suppose $|S_i| = \ell$. Let $e_1, \ldots, e_\ell$ be
the elements of $S_i$ and suppose $e_k$ was the $k$ th element of $S_i$
to be covered by @alg-greedy-setcover.

Consider the iteration in which $e_k$ was covered. At the start of the
iteration, $e_k$ was uncovered and so the set $S_i$ was not yet chosen
and is a candidate for this iteration. Moreover, $e_k, \ldots, e_\ell$
were uncovered and so $|S_i \setminus C| \geq \ell - k +1$.

Therefore, by the greedy choice, a set $S_j$ containing $e_k$ with
$\frac{w_j}{|S_j \setminus C|} \leq \frac{w_i}{|S_i \setminus C|}$ was
chosen and thus
$$y_{e_k} \leq \frac{w_i}{|S_i \setminus C|} \leq \frac{w_i}{\ell - k + 1}.$$

We have $$\sum_{e \in S_i} y_e
\leq \sum_{k=1}^\ell \frac{w_i}{\ell - k + 1}
= \sum_{k=1}^\ell \frac{w_i}{k} = H_\ell \cdot w_i \leq H_n \cdot w_i,$$
as desired.

:::
