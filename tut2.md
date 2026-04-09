(sec-tut2)=

# Tutorial 2: Matroids

::: {exercise label=ex-2-1}

Show that the [uniform matroid](#unif-matroid) and [partition
matroid](#part-matroid) satisfy the augmentation property.

:::

::: {solution class=dropdown} ex-2-1

****Uniform matroid.**** Let $M = (E,\mathcal{I})$ be a uniform matroid
where the independent sets are sets of size at most $k$. Let $S$ and $T$
be subsets of $E$ such that $k \geq |S| > |T|$. Then, for any element
$e$ in $S \setminus T$, we have $|T \cup \{e\}| \leq |S| \leq k$ and so
$|T \cup \{e\}|$ is independent, as desired.

****Partition matroid.**** Suppose $M = (E, \mathcal{I})$ is a partition
matroid with partition $E_1, \ldots, E_m$ and capacities $k_i$ for each
part $E_i$. Let $S$ and $T$ be independent sets with $|S| > |T|$. Since
$|S| > |T|$, there exists a part $E_i$ such that
$k_i \geq |S \cap E_i| > |T \cap E_i|$. Pick any element $e$ in
$(S \cap E_i) \setminus (T \cap E_i)$, and let $T' = T \cup \{e\}$. We
have
$$|T' \cap E_j| = |T \cap E_j| \leq k_j \qquad \text{for $j\neq i$}$$
and
$$|T' \cap E_i| \leq |S \cap E_i| \leq k_i \qquad \text{for $j = i$.}$$
Thus, $T'$ is independent, as desired.

:::

::: {exercise}

Convince yourself that when the [greedy algorithm for
matroids](#alg-matroid-greedy) is applied to graphical matroids, it is
exactly [Kruskal's algorithm](#alg-Kruskal), and that its [proof of
correctness](#prf-greedy-matroid) can be easily modified[^1] to give a
proof of correctness for Kruskal's.

:::

::: {exercise label=ex-2-3}

Consider matroid $M = (E, \mathcal{I})$. Suppose you ask \<insert your
favorite LLM\> to give you a basis. After a while, it returns a subset
$S \subseteq E$. Being the savvy person that you are, you want to check
if $S$ is a basis and you want to do so using only $O(n)$ time, assuming
checking independence takes $O(1)$ time.

Your task is to show that checking if $S$ is a basis can be done in
$O(n)$ time.

:::

::: {solution class=dropdown} ex-2-3

We use the [observation](#obs-basis-circuit) that by definition, an
independent set is a basis if and only if it is maximally independent.
First, we check if $S$ is independent; if not, then $S$ cannot be a
basis. Then for every element $e \notin S$, we check if $S \cup \{e\}$
is independent; if any of them is independent, then $S$ is not a basis.

:::

::: {exercise label=ex-2-4}

Consider a variant of the [Scheduling with Deadlines
Problem](#prob-sched-deadlines) where each job $j$ has a penalty $q_j$
that is incurred if it is not completed on time, instead of a profit for
on-time completion, and the goal is to find an ordering of jobs that
minimizes the total penalty incurred.

1.  (Warm up) One idea is to convert an instance $I$ of the variant into
    an instance $I'$ of the [Scheduling with Deadlines
    Problem](#prob-sched-deadlines) with the same set of jobs and
    deadlines but job $j$ has profit $-q_j$. Show that this idea does
    not work, i.e. there is an instance of the variant $I$ such that the
    optimal solution for $I'$ is not the same as the optimal solution
    for $I$.
2.  Your task is to reformulate this problem into a max-weight or
    min-weight basis problem.

:::

::: {solution class=dropdown} ex-2-4

****Part 1.**** Let $I$ be an instance of the min-penalty variant with
two jobs:

- Job 1 has deadline 1 and penalty 1
- Job 2 has deadline 1 and penalty 2

The optimal solution schedules job 2 first and incurs a total penalty of
1.

On the other hand, in the max-profit instance $I'$, we have:

- Job 1 has deadline 1 and profit -1
- Job 2 has deadline 1 and profit -2

Thus, the optimal solution schedules job 1 first and obtains a total
profit of -1.

****Part 2.**** This problem is actually equivalent to the original
Scheduling with Deadlines Problem since maximizing the sum of $q_j$ of
jobs completed on time is equivalent to minimizing the sum of $q_j$ of
jobs not completed on time.

More precisely, consider an instance $I$ of the variant with penalties
$q_j$ and an instance $I'$ of the original problem with the same jobs
and deadlines but job $j$ has profit $q_j$. Consider an ordering of jobs
in which the subset of jobs $S$ is completed on-time. The penalty
incurred is exactly $\sum_{j \notin S} q_j$ while the profit obtained is
$\sum_{j \in S} q_j = \sum_j q_j - \sum_{j \notin S} q_j$. Thus, the
min-penalty variant is equivalent to the original max-profit problem.

:::

More generally, choosing a subset with maximum weight is equivalent to
choosing a subset whose complement has minimum weight.

::: {prf:theorem label=thm-complement-trick}

Let $U$ be a ground set and $\mathcal{F}$ be a collection of feasible
sets. For every weight function $w$, the set $S^*$ is an optimal
solution for $$\max_{S \in \mathcal{F}} w(S)$$ if and only if $S^*$ is
also an optimal solution for
$$\min_{S \in \mathcal{F}} w(U \setminus S).$$

:::

For the following problem, we will need a similar theorem.

::: {prf:proof enumerate=false}

For every subset $S \subseteq U$, we have
$w(S) + w(U \setminus S) = w(U)$. Thus, for any two subsets
$S, T \subseteq U$, we have $w(S) > w(T)$ if and only if
$w(U \setminus S) < w(U \setminus T)$, and so the optimal solution for
one is also optimal for the other.

:::

::: {prf:theorem label=thm-complement-trick-2}

Let $U$ be a ground set and $\mathcal{F}$ be a collection of feasible
sets. Define $\mathcal{F}'$ to be the collection of sets $R$ such that
$U \setminus R \in \mathcal{F}$, i.e. the complement of $R$ is a
feasible set for $\mathcal{F}$.

Then, for every weight function $w$, a set $S^*$ is an optimal solution
for $$\max_{S \in \mathcal{F}} w(S)$$ if and only if $U \setminus S^*$
is an optimal solution for $$\min_{R \in \mathcal{F}'} w(R).$$

:::

::: {prf:proof enumerate=false}

The theorem follows from @thm-complement-trick and the fact that
$S \in \mathcal{F}$ if and only if $U \setminus S \in \mathcal{F}'$.

:::

::: {exercise label=ex-2-5}

We are given a set of $n$ items $e_1, \ldots, e_n$ with weights $w(e_i)$
and priorities $p(e_i)$. The priority of item $i$ is an integer such
that $1 \leq p(e_j) \leq n$. Moreover, for each priority
$1 \leq \ell \leq n$, we are given a requirement $k_\ell$. The goal is
to find a min-weight subset of items $S$ such that for each priority
$1 \leq \ell \leq n$, there at least $k_\ell$ items in $S$ with priority
at least $\ell$.

Your task is to reformulate this problem into a max-weight or min-weight
basis problem.

:::

::: {solution class=dropdown} ex-2-5

Let $U$ be the set of $n$ items. By @thm-complement-trick-2, the problem
is equivalent to finding a max-weight set of items $R$ such that
$U \setminus R$ has at least $k_\ell$ items with priority at least
$\ell$, for every $1 \leq \ell \leq n$.

For each integer $1 \leq \ell \leq n+1$, define the set $T_\ell$ to be
the set of items $e_i$ with priority $p(e_i) \geq \ell$. Observe that
$T_1 = U$ and $T_{n+1} = \emptyset$.

We say that a set $S$ is *feasible* if $|S \cap T_\ell| \geq k_\ell$ for
every $1 \leq \ell \leq n$, and we say that a set $R$ is *removable* if
$U \setminus R$ is feasible. Thus, $R$ is removable if and only if
$|R \cap T_\ell| \leq |T_\ell| - k_\ell$ for every $1 \leq \ell \leq n$.

We now show that the removable sets form a [matroid](#def-matroid) using
a similar [proof](#prf-sched-matroid) as the one for Scheduling with
Deadlines. It is easy to see that they satisfy the empty set and
downwards-closed properties so it remains to show that they satisfy the
augmentation property. Let $Q$ and $R$ be removable sets such that
$|Q| > |R|$. Then, let $\ell^*$ be the smallest integer strictly greater
than 1 such that $$|Q \cap T_{\ell^*}| \leq |R \cap T_{\ell^*}|.$$ Such
an integer exists since $$|Q \cap T_1| = |Q| > |R| = |R \cap T_{1}|$$
and $$|Q \cap T_{n+1}| = 0 = |R \cap T_{n+1}|.$$

Let $i^*$ be an item in
$(Q \cap T_{\ell^*-1}) \setminus (R \cap T_{\ell^*-1})$ and define
$R' = R \cup \{i^*\}$. We now show that $R'$ is removable. For
$\ell \leq \ell^*-1$, we have $|R \cap T_{\ell}| < |Q \cap T_{\ell}|$
and so $|R' \cap T_{\ell}| \leq |Q \cap T_{\ell}| \leq k_\ell$. For
$\ell > \ell^*$, we have $R' \cap T_\ell = R \cap T_\ell$ and so
$|R' \cap T_\ell| \leq k_\ell$. We conclude that $R'$ is removable.
Therefore, the removable sets satisfy the augmentation property and form
a matroid.

We conclude that we have reformulated the problem into a max-weight
basis problem.

:::

[^1]: For example, by substituting the matroid terms with the
    appropriate graph terms.
