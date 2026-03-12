(sec-tut2)=

# Tutorial 2: Matroids

::: {exercise}

Show that the [uniform matroid](#unif-matroid) and [partition
matroid](#part-matroid) satisfy the augmentation property.

:::

::: {exercise}

Convince yourself that when the [greedy algorithm for
matroids](#alg-matroid-greedy) is applied to graphical matroids, it is
exactly [Kruskal's algorithm](#alg-Kruskal), and that its proof of
correctness can be easily modified[^1] to give a proof of correctness
for Kruskal's.

:::

::: {exercise}

Consider an instance of the Max-Weight Basis Problem with matroid
$M = (E, \mathcal{I})$ and weight function $w$. Suppose you ask \<insert
your favorite LLM\> to give you a max-weight basis. After a while, it
returns a subset $S \subseteq E$. Being the savvy person that you are,
you want to check if $S$ is a max-weight basis and you want to do so
using only $O(n)$ time, assuming checking independence takes $O(1)$
time.[^2]

1.  (Warm up) Show that checking if $S$ is a basis can be done in $O(n)$
    time.
2.  Show that checking if $S$ is a max-weight basis can be done in
    $O(n)$ time.

:::

::: {exercise}

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

::: {exercise}

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

[^1]: For example, by substituting the matroid terms with the
    appropriate graph terms.

[^2]: If you take $O(n \log n)$ time, then you might as well run the
    greedy algorithm for matroids yourself, which defeats the purpose of
    asking the LLM.
