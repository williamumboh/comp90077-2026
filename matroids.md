(sec-matroids)=

# Matroids

A key goal in algorithms research is to discover general mathematical
principles that apply to broad classes of algorithms and problems. To
this end, it is useful to define more formally what we mean by a
combinatorial optimization problem. At an intuitive level, in a
combinatorial optimization problem, we need to choose from a finite set
of objects that maximizes or minimizes some function and that satisfy
some constraints.

::: {prf:definition label=prob-comb} Combinatorial Optimization Problem

A combinatorial optimization problem consists of:

- a finite set of elements $E$;
- an *objective function* $g : 2^E \rightarrow \mathbb{R}$ (i.e. $g$ is
  a function that maps subsets of $E$ to real numbers);
- a collection $\mathcal{F} \subseteq 2^E$ of subsets of \$E. The sets
  in $\mathcal{F}$ are called *feasible sets*.

The goal is to find a feasible set $S \in \mathcal{F}$ that maximizes or
minimizes $g(S)$.

:::

A common class of objective functions involve summing up the weights of
the elements in the subset. These are called additive functions.

::: {prf:definition} Additive Functions (Sum of Weights)

A function $g: 2^E \rightarrow \mathbb{R}$ is an *additive function* if
there exists a weight function $w: E \rightarrow \mathbb{R}$ such that
for every subset $S \subseteq E$, we have $g(S) =\sum_{e \in S} w(e)$.

:::

::: {prf:example} Minimum Spanning Trees

In the Minimum Spanning Tree problem, the set of elements is the set of
edges, the objective function $g(S)$ is the total weight of edges in
$S$, and $\mathcal{F}$ is the set of spanning trees.

:::

Thus, we can restate @thm-kruskal as follows.

::: {prf:theorem}

If $\mathcal{F}$ is the set of spanning trees of a graph and $g$ is
additive function, then [Kruskal's algorithm](#alg-Kruskal) finds an
optimal solution.

:::

Many greedy algorithms (such as [Kruskal's algorithm for
MST](#alg-Kruskal)) share the same "exchange argument" in their
correctness proofs. Thus, it is natural to ask what properties of $g$
and $\mathcal{F}$ enable a greedy algorithm. We now introduce *matroids*
as a generalization of spanning trees[^1] and a greedy algorithm that
generalizes [Kruskal's algorithm for MST](#alg-Kruskal). We will see
that matroids capture, in a minimal manner, the properties of spanning
trees that enable a greedy algorithm.

## Independence Systems

We start by observing that one useful feature of acyclic subgraphs is
that they are downwards-closed: if a subset of edges $F$ has no cycles,
then every subset of $F$ also has no cycles. This motivates the
definition of independence systems.

::: {prf:definition label=def-indep} Independence Systems

An *independence system* $M = (E, \mathcal{I})$ [^2] consists of a
finite set of elements $E$ (called the *ground set* of $M$) and a
collection $\mathcal{I}$ of subsets of $E$ (these are called the
*independent sets* of $M$) satisfying the following two properties (also
called *axioms*):

- ****Empty set:**** The empty set is independent, i.e.
  $\emptyset \in \mathcal{I}$.
- ****Downwards-closed:**** If $S$ is independent, then every subset of
  $S$ is also independent.

:::

Many familiar combinatorial optimization problems can be cast as a
problem of finding the maximum-weight independent set.

::: {prf:definition label=def-knapsack} Knapsack Problem

We are given $n$ items $e_1, \ldots, e_n$ with weights $w(e_i)$ and
sizes $s(e_i)$, and a positive integer $C$ called the knapsack capacity.
The goal is to find a maximum-weight set of items whose sizes sum to at
most $C$, i.e. a subset $A$ maximizing $w(A)$ subject to $s(A) \leq C$.

:::

It is easy to see that the collection of feasible sets for the knapsack
problem, i.e. subsets whose total size is at most $C$, satisfy the [two
properties of an independence system](#def-indep).

## Matroids

Matroids are independent systems with an additional property.

::: {prf:definition label=def-matroid} Matroids

An independence system $M$ is a *matroid* if it also satisfies the
following property:

- ****Augmentation:**** If $S$ and $T$ are independent and $|S| > |T|$,
  then there is an element $e \in S \setminus T$ such that
  $T \cup \{e\}$ is also independent.

:::

Let us now see how matroids generalize spanning trees.

::: {prf:definition} Graphical Matroid

Let $G = (V,E)$ be a graph and consider the matroid $M_G$ whose ground
set is the edge set $E$ of $G$, and whose independent sets are exactly
the acyclic subgraphs (aka forests) of $G$. That is, a subset of edges
is independent if they do not contain a cycle. The matroid $M_G$ is
called a *graphical matroid*.

:::

Observe that $M_G$ indeed satisfies the 3 matroid properties:

- The empty subgraph does not contain a cycle.
- If a subgraph does not contain a cycle, then any subset of it does not
  contain a cycle either.
- The augmentation property is proved in @ex-1-2.

Next, we define the analog of spanning trees. An independent set $B$ is
called a *basis* if it is maximally independent, i.e. there is no
independent set $B'$ that contains $B$ and $|B'| > |B|$. The *rank* of a
subset $S$ of the ground set is the size of the largest independent set
contained in $S$, and is denoted by $r(S)$. The *rank* of the matroid is
$r(E)$, the size of the largest independent set. Let $T$ be a subset of
the ground set, and $S$ be an independent set contained in $T$. Then,
$S$ is said to be a *maximally independent subset* of $T$ if there is no
independent set $S'$ such that $S \subsetneq S' \subseteq T$.

::: {prf:example} Bases in Graphical Matroids

Observe that the bases in a graphical matroid $M_G$ is exactly the
spanning trees of $G$. The following proposition implies that all
spanning trees have the same size.

:::

::: {prf:proposition label=size-bases}

For every subset $T$ of the ground set, every maximally independent
subset of $T$ has the same size, $r(T)$. In particular, every base has
the same size $r(E)$.

:::

::: {prf:proof enumerated=false}

Suppose $S_1$ and $S_2$ are contained in $T$ and independent. If
$|S_1| > |S_2|$ then the [augmentation property](#def-matroid) implies
that there exists an element $e \in S_1 \setminus S_2$ such that
$S_2 \cup \{e\}$ is independent. Since $e \in T$, we get that
$S_2 \cup \{e\}$ is also contained in $T$. Therefore, $S_2$ is not a
maximally independent subset of $T$.

:::

Finally, we consider the analog of cycles. A subset $S$ that is not
independent is called *dependent* and is called a *circuit* if it is
minimally dependent, i.e. it has no proper subset that is also
dependent.

::: {prf:example} Circuits in Graphical Matroids

A dependent subset of a graphical matroid $M_G$ corresponds to an edge
subset of $G$ that contains a cycle. A circuit of $M_G$ then corresponds
to a cycle in $G$.

:::

The following observation about bases and circuits follow immediately
from their definition and is useful.

::: {prf:observation}

An independent set $S$ is a basis if and only if adding any element to
$S$ makes it dependent. A dependent set $S$ is a circuit if and only if
removing any element from $S$ yields an independent set.

:::

## Examples of Matroids

The simplest matroid is a uniform matroid.

::: {prf:definition label=unif-matroid} Uniform Matroid

A uniform matroid is specified by a ground set $E$ and a non-negative
integer $k$. A set $S$ is independent if $|S| \leq k$.

:::

::: {prf:definition label=part-matroid} Partition Matroid

A partition matroid is specified by a ground set $E$, a partition of the
ground set into parts $E_1, \ldots, E_m$, and a non-negative integer
$k_i$ for each part. A set $S$ is independent if for each part $E_i$,
$S$ contains at most $k_i$ elements from $E_i$, i.e.
$|S \cap E_i| \leq k_i$.

:::

## Optimizing over Matroids

We are now ready to define the following matroid optimization problem,
which generalizes the MST problem.

::: {prf:definition label=prob-max-basis} Max-Weight Basis Problem

The input is a matroid $M = (E, \mathcal{I})$ and a weight function $w$
on $E$ where each element can have positive, zero, or negative weight.
The goal is to find a basis $S$ with maximum total weight
$\sum_{e \in S} w(e)$.

:::

In the parlance of @prob-comb, the Max-Weight Basis Problem is a
combinatorial optimization problem where the set of elements is the
ground set of a matroid $M$, and the feasible sets are the bases of the
matroid, and the objective function is such that $f(S)$ is the sum of
weights in $S$.

::: {exercise}

How can we frame the [Minimum Spanning Tree Problem](#prob-MST) as a
[Max-Weight Basis Problem](#prob-max-basis)?

:::

The greedy algorithm for matroids naturally generalizes [Kruskal's
greedy algorithm for MST](#alg-kruskal). We start with the empty set and
add elements in decreasing order of weight as long as doing so preserves
independence.

::: {prf:algorithm label=alg-matroid-greedy} Greedy Max-Weight Basis

- Initialize $B \leftarrow \emptyset$
- ****for each**** $e \in E$ in decreasing order of weight ****do****
  - Add $e$ to $B$ if $B \cup \{e\}$ is independent and discard $e$
    otherwise
- ****end for****
- ****return**** $B$

:::

Henceforth, we use $w(S)$ to mean $\sum_{e \in S} w_e$.

::: {prf:theorem label=thm-greedy-matroid} Greedy Optimality

Let $M$ be a matroid. Then, for every weight function $w$ on the
elements of $M$, the [greedy algorithm](#alg-matroid-greedy) finds the
maximum-weight basis in $O(n \log n)$ time, assuming an independence
oracle[^3]. If checking a given subset is independent takes $g(n)$ time,
then the greedy algorithm takes $O(n \log n + n\cdot g(n))$ time.

:::

::: {prf:proof enumerated=false}

Let $B$ be the output of the greedy algorithm and $T$ be a
maximum-weight basis.

First, observe that $B$ is a basis: if $B$ is not a basis, then the
[augmentation property](#def-matroid) implies that there is an element
$e \notin B$ such that $B \cup \{e\}$ is independent, and so the
algorithm would have added $e$ since it considers all elements. Since
every base has the same size, $|B| = |T|$.

Next, we use the [augmentation property](#def-matroids) and an exchange
argument similar to the proof of @ex-1-3 to prove that $w(B) = w(T)$.
Let $k = |B| = |T|$. Suppose the elements of $B$ are $b_1, \ldots, b_k$,
and the elements of $T$ are $t_1, \ldots, t_k$, both sorted in
decreasing order of weight.

Suppose, towards a contradiction, that $w(B) < w(T)$. Let $i$ be the
smallest index such that $w(b_i) < w(t_i)$ and consider the independent
sets $$B' = \{b_1, \ldots, b_{i-1}\}$$ and
$$T' = \{t_1, \ldots, t_{i-1},t_i\}.$$

By the [augmentation property](#def-matroids), there is some element
$t_j \in T' \setminus B'$ such that $B' \cup \{t_j\}$ is independent.
Since the elements of $B$ and $T$ are sorted in decreasing order, we
have $w(t_j) \geq w(t_i) > w(b_i)$.

Consider the iteration in which the algorithm considered the element
$t_j$. Since $w(t_j) > w(b_i)$, this iteration is before it considered
$b_i$. Thus, at this point in time, $B$ is a subset of $B'$ and so by
the [downwards-closed property](#def-matroids), $B \cup \{t_j\}$ is also
independent. Therefore, the algorithm would have added $t_j$ to its
solution. On the other hand, since $t_j \notin B'$ means that by the
time the algorithm got to $b_i$, it had considered $t_j$ and had
discarded it. Contradiction.

In conclusion, the assumption that $w(B) < w(T)$ led to a contradiction,
and thus, it must be the case that $w(B) = w(T)$.

As for the running time, observe that we need $O(n \log n)$ time to sort
and then we perform $n$ independence checks.

:::

## What about independence systems?

The description of @alg-matroid-greedy only relies on the notion of
independence so one may wonder if @thm-greedy-matroid finds the
max-weight independent set in *every* [independence
system](#def-matroids), not just matroids. Is greedy still optimal even
if we do not have the augmentation property? It turns out that's too
good to be true.

::: {prf:theorem label=thm-greedy-indep}

There exists an independence system in which @alg-matroid-greedy fails
to find the max-weight independent set.

:::

Before we go into the proof, it is important to discuss how to go about
proving such a statement. To prove that greedy is not optimal in every
independence system, we only need to provide a counter-example, i.e. to
give an independence system for which greedy fails. So far, the only
independence system that is not a matroid that we have seen is the one
defined by the [Knapsack Problem](#def-knapsack). As this is one of the
famous NP-complete problems and the greedy algorithm is a
polynomial-time algorithm, intuitively, one should be able to come up
with a counter-example by constructing a suitable instance of the
Knapsack Problem.

::: {prf:proof enumerated=false}

As the feasible sets of an instance of the Knapsack Problem forms an
independence system, it suffices to construct an instance of the
Knapsack Problem. Since the greedy algorithm starts by picking the
highest-weight item, we want to construct an instance in which picking
the highest-weight item is a bad idea. Intuitively, we do not want to
pick the highest-weight item if we can instead fit many smaller-weight
items.

Consider an instance of the knapsack problem where element $e_1$ has
weight $w(e_1) = 2$ and size $n$, every other element has weight $1$ and
size $1$, and $C = n$. @alg-matroid-greedy will first pick $e_1$ and is
then unable to pick anything else. So, it returns the set $\{e_1\}$
which has weight $2$. On the other hand, the set $E \setminus \{e_1\}$
has total size $n-1 \leq C$ and has weight $n-1$. Thus, greedy not only
fails to find the optimal solution, but is off by a factor of
$\Omega(n)$.

:::

@thm-greedy-matroid and @thm-greedy-indep give a powerful
characterization of matroids in terms of greedy.

::: {prf:theorem label=thm-matroids} Greedy Characterization of Matroids

Let $M$ be an independence system. @alg-matroid-greedy finds the
max-weight independent set for every weight function $w$ if and only if
$M$ is a [matroid](#def-matroid).

:::

How do we use this Theorem? We can use it in 2 ways:

1.  To show that @alg-matroid-greedy works for an independence system
    $M$, it suffices to show that $M$ is a matroid, i.e. it satisfies
    the augmentation property.
2.  To show that @alg-matroid-greedy does not work for $M$, it suffices
    to show that $M$ is not a matroid, i.e. it does not satisfy the
    augmentation property.

## Application: Scheduling with Deadlines

To apply @thm-greedy-matroid to a specific [combinatorial optimization
problem](#def-comb), we need to show that the feasible sets of the
problem form a [matroid](#def-matroid). Let us see this in action for
the following scheduling problem.

::: {prf:definition label=prob-sched-deadlines} Scheduling w/ Deadlines

We are given a set of $n$ jobs labeled $1, \ldots, n$. Every job
requires one day of processing time. Each job $j$ has a positive integer
deadline $1 \leq d_j \leq n$ and a non-negative profit $p(j)$ that is
earned if $j$ is completed by the end of day $d_j$. The goal is to
schedule the jobs, i.e. decide in which order to perform the jobs, to
maximize the total profit earned.

:::

::: {prf:example}

Suppose we have 3 jobs:

- Job 1 has deadline 1 and profit 5;
- Job 2 has deadline 2 and profit 10;
- Job 3 has deadline 2 and profit 1.

Then, if we perform the jobs in the order 1, 2, 3, we get the profit for
jobs 1 and 2. Observe that it is not possible to complete all 3 jobs on
time.

:::

At first glance, it is not clear what this has to do with matroids. In
matroids, we are concerned with subsets of elements while this problem
is concerned with finding an ordering. To turn this into a matroid
optimization problem, we need to reformulate the problem.

Say that a subset of jobs $S$ is *feasible* if there is an ordering in
which all of them are completed on time. Thus, instead of finding a
max-profit ordering of jobs, we can instead solve the equivalent problem
of finding a max-profit subset of jobs that is feasible.

Our goal is to show that the feasible subsets of jobs form a matroid.
First, we prove a characterization of feasible subsets.

::: {prf:lemma label=lem-feasible-jobs} Characterization of Feasibility

Let $S$ be a set of jobs and $S(t)$ be the subset of jobs $j \in S$ with
deadline $d_j \leq t$. We have that $S$ is feasible if and only if
$|S(t)| \leq t$ for every $t$.

:::

:::::: {prf:proof enumerated=false}

Suppose $|S(t)| > t$ for some $t$. Then no matter how we order the jobs,
at least one job $j \in S(t)$ will finish on day $t+1$. Since every job
in $S(t)$ has deadline at most $t$, there is some job that cannot be
completed on time. Thus, $S$ is not feasible.

Suppose $|S(t)| \leq t$ for every $t$. Schedule $S$ in increasing order
of deadlines and order the remaining jobs arbitrarily after the last job
in $S$. Each job $j \in S$ is completed by the end of day $|S(d_j)|$ and
so is completed on time since $|S(d_j)| \leq d_j$.

::::::

::: {aside}

In this proof, we use the [3rd approach for proving if and only if
statements](#sec-iff).

:::

::: {prf:lemma label=lem-sched-matroid}

The feasible subsets of jobs form a matroid.

:::

::: {prf:proof enumerated=false}

Let $\mathcal{F}$ be the collection of feasible subsets of jobs.

The characterization of feasible subsets (@lem-feasible-jobs)
immediately implies that $\mathcal{F}$ satisfies the [empty set and
downwards-closed properties](#def-matroid) of matroids.

It remains to prove that $\mathcal{F}$ satisfies the augmentation
property. Let $R$ and $S$ be two feasible subsets such that $|R| > |S|$.
We need to find a job $j \in R \setminus S$ such that $S \cup \{j\}$ is
feasible.

Let $t^*$ be the latest day such that $|R(t^*)| \leq |S(t^*)|$. Such a
day exists since $$|R(0)|=0=|S(0)|$$ and
$$|R(n)| = |R| > |S| = |S(n)|.$$ Observe that $|R(t)| > |S(t)|$ for
every $t>t^*$. Thus, we have $|R(t^*+1)| > |S(t^*+1)|$ and so there is a
job $j^* \in R(t^*) \setminus S(t^*)$ whose deadline is $t^* + 1$. We
now show that $S' = S \cup \{j^*\}$ is feasible. By @lem-feasible-jobs,
it suffices to show that $|S'(t)| \leq t$ for every $t$.

****Case 1 ($t \leq t^*$).**** We have that $S'(t) = S(t)$ since the new
job $j^*$ has deadline $t^* > t$ and so is not in $S(t)$; thus
$|S'(t)| \leq t$ by feasibility of $S$.

****Case 2 ($t > t^*$).**** As argued above, we have $|S(t)| < |R(t)|$
when $t > t^*$. Thus, $$|S'(t)| = |S(t)| + 1 \leq |R(t)| \leq t$$
because $R$ is feasible.

Therefore, $S'$ is feasible. We have now proved that $\mathcal{F}$
satisfies the augmentation property and we conclude that $\mathcal{F}$
is a matroid.

:::

@lem-sched-matroid and @thm-matroids now imply the following theorem.

::: {prf:theorem}

There is a polynomial-time algorithm that finds the optimal ordering of
jobs for every instance of the Scheduling with Deadlines Problem.

:::

[^1]: If you are familiar with linear algebra, matroids was also
    introduced to generalize combinatorial aspects of vectors. This is
    why we use terminology such as independent sets, bases, and ranks.

[^2]: The symbol $\mathcal{I}$ is "calligraphic" font. Typically,
    lower-case letters refer to elements, upper-case letters refer to
    sets, and upper-case letters in calligraphic font refer to
    collections of sets. For example, when talking about graphs, we use
    $e$ to refer to an edge and $E$ to refer to a set of edges. If we
    have several sets of edges $E_1, E_2, \ldots, E_k$, we can write
    $\mathcal{E} = \{E_1, E_2, \ldots, E_k\}$.

[^3]: This means that we can check if a subset $S$ is independent in
    $O(1)$ time. Informally, one can think of an independence oracle as
    an external API call that returns an answer instantly.
