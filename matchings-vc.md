# Matchings and Vertex Covers

Today is going to be a warmup for some of the key concepts in linear
programming. Just like matroids and submodularity, linear programming
can look very abstract the first time you see it so we are going to ease
into it by starting with the concrete graph problems of matchings and
vertex covers. We will also see a systematic way of approaching the task
of designing algorithms for new problems.

Matchings and vertex covers are both graph problems.

## Matchings

::: {prf:definition label=def-matching} Matching

Let $G = (V,E)$ be a graph. A subset of edges $F$ is a *matching* if
every vertex $v \in V$ is incident to at most one edge of $F$. Moreover,
$F$ is a *maximal matching* if there is no strict superset
$F' \supsetneq F$ that is a matching, [^1] and $F$ is a *perfect
matching* if every vertex $v \in V$ is incident to **exactly one** edge
of $F$.

:::

There are several natural optimization problems involving matchings. The
one that we will be focusing on today is the Maximum Matching Problem.

::: {prf:definition label=prob-matching} Maximum Matching

Given a graph $G = (V,E)$, the goal is to find a matching $M$ that
maximizes $|M|$.

:::

::: {prf:observation label=maximal-vs-maximum} Maximal vs Maximum

Every maximum matching is maximal, but not every maximal matching is
maximum.

:::

::: {prf:proof enumerated=false}

Consider the graph with vertices $v_1, v_2, v_3, v_4$ and edges
$(v_1, v_2), (v_2, v_3), (v_3, v_4)$. Then, $M = \{(v_2,v_3)\}$ is
maximal but not maximum as $M^* = \{(v_1,v_2), (v_3, v_4)\}$ is larger
matching.

:::

An important special case of the Maximum Matching Problem is when $G$ is
bipartite. We call this the Maximum Bipartite Matching Problem.

::: {prf:definition label=def-bipartite} Bipartite Graph

A *bipartite graph* $G = (V,E)$ is one whose vertex set $V$ can be
partitioned into two sets $L$ and $R$ (called *left part* and *right
part*, respectively) such that every edge in $E$ has exactly one
endpoint in $L$ and the other in $R$, i.e. there is no edge between two
vertices in the same part.

:::

### Applications

There are many practical applications of the Maximum Matching Problem.
Here is one in job scheduling: Suppose we are given a set of jobs that
we want to schedule on a set of machines. Each machine can only run at
most one job, and each job $j$ can only be assigned to a subset of
machines. The goal is to assign as many jobs as we can to machines. We
can model this as a Maximum Bipartite Matching Problem where we have a
vertex $v_i$ for each machine $i$, a vertex $v_j$ for each job $j$, and
an edge between a machine vertex $v_i$ and a job vertex $v_j$ if job $j$
can be assigned to machine $i$.

Other examples include: matching Uber drivers with riders, kidney
exchange, and ad auctions.

## Initial Attempts

Looking back at the analyses of the algorithms we have seen so far, a
common pattern emerges: there is some structural property that is
leveraged to design an algorithm. For example, the
[proof](#prf-greedy-matroid) that the [greedy algorithm is optimal for
Maximum-Weight Basis Problem](#thm-greedy-matroid) crucially relies on
the [matroid](#def-matroid) properties of downwards-closedness and
augmentation.

Thus, the first phase of algorithm design is to get a feel for the
problem and find structural properties that can be exploited
algorithmically. Indeed, in general, instead of thinking in binary
terms—either we have an algorithm that works or not—a better idea is to
reframe progress in terms of how much we have learnt about the problem.

To get a feel for the problem, it's often a good idea to restrict our
attention to the simplest possible version of the problem. In this case,
we want to look at Maximum Bipartite Matching, instead of Maximum
Weighted Matching in general graphs. This has several benefits:

- bipartite graphs are much easier to draw and visualise, allowing us to
  involve the visual part of our brain
- reduces the space of potential properties and algorithms to search for
  (e.g. edge weights add the complication that it may be possible for a
  matching $M$ to have larger weight than $M'$ even though $|M|$ is much
  smaller than $|M'|$)
- psychologically, starting with the simplest version increases our
  likelihood of making progress, which provides dopamine and confidence
  boost that is crucial to maintaining our motivation.

Now that we have restricted our attention to Maximum Bipartite Matching,
let us see if it is one of the problem types that we have seen before.
The first problem type is matroid. It is clear that matchings in a
bipartite graph satisfy downwards-closedness. Since a [maximal matching
is not necessarily maximum](#maximal-vs-maximum) and the augmentation
property implies that maximal independent sets are maximum independent
sets, matchings do not satisfy the augmentation property. Thus,
matchings form an independence system but not a matroid.

### What about Greedy?

Often, a natural first algorithm to try is a greedy algorithm. Here's a
natural greedy algorithm.

::: {prf:algorithm label=alg-greedy-matching} Greedy Matching

- Initialize $M \leftarrow \emptyset$
- ****for each**** $e \in E$ ****do****
  - Add $e$ to $M$ if $M \cup \{e\}$ is a matching and discard $e$
    otherwise
- ****end for****
- ****return**** $M$

:::

Since we have showed that matchings form an independence system but not
a matroid, by the [Greedy Characterization of Matroids](#thm-matroids),
we get that greedy is sub-optimal. In particular, observe that if we run
Greedy on the example in @maximal-vs-maximum, if Greedy first picks
$(v_2,v_3)$, then it cannot pick anything else.

::: {important}

Note that Greedy finds the maximum matching if it goes through $E$ in
the right order, in particular, if the edges of the maximum matching
appear first in the order. However, in worst-case analysis, **we assume
the worst case when breaking ties**. Thus, we say that Greedy is
sub-optimal since there is some ordering in which its matching is not
maximum.

:::

Next, we will show that while Greedy is sub-optimal, its approximation
ratio is $1/2$, in other words, it always (regardless of the order in
which it goes through $E$) finds a matching of size at least half the
maximum. To do this, we will need to define the notion of a vertex
cover.

## Vertex Covers

::: {prf:definition label=def-vc} Vertex Cover

Let $G = (V,E)$ be a graph. A vertex subset $C \subseteq V$ is a *vertex
cover* if for every edge $(u,v) \in E$, either $u$ or $v$ is in $C$. In
other words, every edge of the graph is incident to some vertex in $C$.

:::

Its associated optimization problem is the following.

::: {prf:definition label=prob-vc} Minimum Vertex Cover

Given a graph $G = (V,E)$, find a vertex cover $C$ that minimizes $|C|$.

:::

Observe that the Vertex Cover Problem is a special case of the [Set
Cover Problem](#prob-set-cover). We can formulate Vertex Cover as Set
Cover as follows: for each edge $e$, we have an element $x(e)$, and for
each vertex $v$, we have a set $S(v)$ which contains element $x(e)$ for
every $e$ incident to $v$. This implies that Density Greedy is a
$O(\log n)$ approximation. As we will shall see later, we can get a much
better $2$-approximation.

## Duality between Matching and Vertex Cover

It turns out that the Maximum Matching and Vertex Cover problems are
"dual" to each other, and duality will allow us to get a single
algorithm that simultaneously finds a $1/2$-approximation for Maximum
Matching and a $2$-approximation for Minimum Vertex Cover. The notion of
"dual" will be formally defined when we discuss linear programming.

::: {prf:theorem label=thm-matching-vc-duality} Duality

For every matching $M$ and every vertex cover $C$, we have
$|M| \leq |C|$.

:::

::: {prf:proof enumerated=false}

Let $F$ be a set of edges and $C$ be a vertex cover. If $|C| < |F|$,
then there must be a vertex $v$ in $C$ that is incident to at least two
vertices of $F$. Therefore, if $M$ is a matching, then $|C| \geq |M|$.

:::

## Greedy Approximation Algorithm

We now use @thm-matching-vc-duality to show that the [Greedy
Matching](#alg-greedy-matching) algorithm is a $2$-approximation. The
structural properties that we will leverage is the fact that the Greedy
Matching algorithm produces a maximal matching and the following lemma
which relates maximal matchings and vertex covers.

::: {prf:lemma label=lem-maximal-matching-cover}

Let $M$ be a maximal matching. Then the endpoints of $M$ is a vertex
cover of size $2|M|$.

:::

::: {prf:proof enumerated=false}

Let $C$ be the endpoints of $M$. We now argue that every edge is
incident to some vertex in $C$. By construction, every edge of $M$ is
incident to two vertices in $C$. Moreover, since $M$ is a maximal
matching, every edge $e$ not in $M$ is incident to an endpoint of some
edge in $M$ and so incident to some vertex in $C$. Thus, we conclude
that $C$ is a vertex cover of size $|C| = 2|M|$.

:::

Next, we show that in fact, every maximal matching is a
$1/2$-approximation of the maximum matching. This also proves that the
[Greedy Matching](#alg-greedy-matching) algorithm is a
$1/2$-approximation since the algorithm produces a maximal matching.

::: {prf:theorem label=thm-maximal-matching-approx}

Every maximal matching is a $1/2$-approximation of the maximum matching.

:::

::: {prf:proof enumerated=false}

Let $M$ be a maximal matching and $M^*$ be a maximum matching. By
@lem-maximal-matching-cover, the set $C$ of endpoints of $M$ is a vertex
cover of size $|C| = 2|M|$. Moreover, @thm-matching-vc-duality implies
that $|M^*| \leq |C|$. Thus, we have
$$| M^* | ≤ | C | = 2 | M |$$
and so $|M| \geq |M^*|/2$, as desired.

:::

Finally, we show that the endpoints of a maximal matching is a
$2$-approximation to the minimum vertex cover.

::: {prf:theorem label=lem-maximal-matching-approx}

Let $M$ be a maximal matching. Then, the set $C$ of endpoints of $M$ is
a $2$-approximation of the minimum vertex cover.

:::

::: {prf:proof enumerated=false}

Let $C^*$ be a minimum vertex cover. By @thm-matching-vc-duality, we
have that $|C^*| \geq |M|$. Since $|C| = 2|M|$, we get that
$|C| \leq 2|C^*|$.

:::

Therefore, to get a $2$-approximate vertex cover, we can simply run the
[Greedy Matching](#alg-greedy-matching) algorithm and take the endpoints
of its matching.

Note that even though we set out to only consider Maximum Bipartite
Matching, none of the above proofs require bipartite graphs, and thus,
our approximation algorithms work on general graphs.

## Duals as Certificates of Optimality

@thm-matching-vc-duality has an interesting application in certifying
optimality. Suppose we outsource the task of computing a maximum
matching in a graph to a third-party. How do we know if the matching $M$
given by the third-party is indeed maximum without computing a maximum
matching ourselves? The third-party can convince us that $M$ is indeed
maximum by providing a vertex cover $C$ of the same size. To verify, we
can simply check that $C$ is a vertex cover and has the same size as
$M$.

Of course, for this to work, we need what's called strong duality:
maximum matching equals minimum vertex cover. It turns out that in
general graphs, we only have weak duality, that the maximum matching is
at most the minimum vertex cover. In bipartite graphs, as we will see in
the next lecture, we do have strong duality, that maximum matching
equals minimum vertex cover.

[^1]: In other words, we cannot add any edge to $F$ and still have a
    matching.
