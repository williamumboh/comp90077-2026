(sec-cover)=

# Knapsack and Set Cover

In previous weeks, we have seen one type of greedy algorithm and this
week, we will see a different one that has many applications.

## Previous Greedy Algorithms

Let us briefly recap what we have covered so far in this subject.

So far, we have considered [combinatorial optimization
problems](#prob-comb-2) in which each instance is of the form
$$\max_{S \in \mathcal{F}} g(S),$$ where $\mathcal{F}$ is the collection
of independent sets in an [independence system](#def-indep).

In Week 2, we considered the setting where $g$ is additive and we showed
that there is a greedy algorithm that is optimal if and only if
$\mathcal{F}$ forms a matroid. In Week 3, we showed that when $g$ is a
monotone submodular function and $\mathcal{F}$ are the subsets of
cardinality at most $k$, then there is also a greedy algorithm that
achieves a good approximation.

Observe that both of these greedy algorithms are special cases of a
greedy "meta-algorithm" that we call *Marginal Greedy*: start with an
empty solution; while there is an element that we can add to our
solution and remain feasible, we add the one that maximizes the marginal
increase in our objective function value.

::: {prf:algorithm label=alg-marginal-greedy} Marginal Greedy

- Initialize $I \leftarrow \emptyset$
- ****while**** there exists an element $e \notin I$ such that
  $I \cup \{e\} \in \mathcal{F}$ ****do****
  - Find $e \notin I$ that maximizes $g(I \cup \{e\}) - g(I)$
  - Add $e$ to $I$
- ****end for****
- ****return**** $I$

:::

## Knapsack Problem

In Week 2, we considered the Knapsack Problem.

::: {prf:definition} Knapsack Problem

We are given $n$ items $e_1, \ldots, e_n$ with weights $w(e_i)$ and
sizes $s(e_i)$, and a positive integer $C$ called the knapsack capacity.
The goal is to find a maximum-weight set of items whose sizes sum to at
most $C$, i.e. a subset $A$ maximizing $w(A) = \sum_{e \in A} w(e)$
subject to $s(A) \leq C$.

:::

We showed that Marginal Greedy is no good for the Knapsack Problem. In
fact, when there is one item with weight 2 and size 1, it's
approximation ratio is $O(1/n)$,[^1] which is considered very poor.

So we need to design a new greedy algorithm. To do so, it's instructive
to look at the bad instance for Marginal Greedy. In the bad instance,
there is one item with weight 2 and size $n$ while the remaining has
weight 1 and size 1. Intuitively, we want to choose items with better
"bang-per-buck": define the *density* of item $e_i$ to be
$w(e_i)/s(e_i)$. Now, for the bad instance, if we greedily items in
decreasing order of density, we get the optimal solution. This gives us
some hope that the following Density Greedy algorithm is good.

::: {prf:definition label=alg-density-greedy} Density Greedy

- Initialize $A \leftarrow \emptyset$
- ****for each**** item $e_i$ in decreasing order of density
  $w(e_i)/s(e_i)$ ****do****
  - **If** $s(e_i) + s(A) \leq C$ **then** add $e_i$ to $A$
  - **Else** **break**
- ****end for****
- ****return**** $A$

:::

Unfortunately, Density Greedy still does not work. Consider an instance
with 2 items $e_1$ and $e_2$ where $w(e_1)=1$, $s(e_1)=1$, $w(e_2)=n-1$
and $s(e_2) = n$. The density of $e_1$ is $1$ and the density of $e_2$
is less than $1$. Thus, Density Greedy picks only $e_1$ but picking
$e_2$ would have been better by a factor of $\Omega(n)$. Thus, Density
Greedy is also terrible!

Looking closer at the bad instance for Density Greedy, we see that
Marginal Greedy would actually have found the optimal solution. What if
we run both Density Greedy and Marginal Greedy and take the best of the
two greedy solutions? It turns out that this idea works. In fact, we do
not need to run Marginal Greedy but we simply need to take the best of
Density Greedy and the single item with maximum weight, i.e. the first
item that Marginal Greedy picks.

::: {prf:definition label=alg-knapsack} Knapsack Algorithm

- Initialize $A \leftarrow \emptyset$
- ****for each**** item $e_i$ in decreasing order of density
  $w(e_i)/s(e_i)$ ****do****
  - **If** $s(e_i) + s(A) \leq C$ **then** add $e_i$ to $A$
  - **Else** **break**
- ****end for****
- Let $e^*$ be item with heaviest weight
- **If** $w(e^*) \geq w(A)$ **then** **return** single item $e^*$
- **Else** **return** $A$

:::

::: {prf:theorem label=thm-knapsack}

The [Knapsack Algorithm](#alg-knapsack) is a (1/2)-approximation for the
Knapsack Problem.

:::

:::::: {prf:proof enumerated=false}

We will show that either $w(A)$ or $w(e^*)$ is a 1/2-approximation by
establishing an upper bound on the weight of the optimal solution and
showing that $w(A) + w(e^*)$ is at least the upper bound.

The upper bound is obtained by considering an easier version of the
problem called the Fractional Knapsack Problem. As the name suggests, in
this problem, we are allowed to pick a fraction of an item. For example,
if we pick 1/2 of item $e_i$, then it contributes $w(e_i)/2$ to the
weight of the solution and $s(e_i)/2$ to the size. Intuitively, if we
are allowed to pick fractions, then we can only get a solution with
higher weight. Moreover, as we show later, the fractional problem is
actually a max-weight uniform matroid basis problem. Thus, we get a
simple characterization of an upper bound on the optimal solution. We
now make this more precise.

Suppose our instance $I$ has items $e_1, \ldots, e_n$, with weights
$w(e_i)$ and sizes $s(e_i)$, and capacity $C$. Suppose that the items
are ordered in decreasing order of density. Define the fractional
instance $I'$ where for each item $e_i$ in $I$, we create $s(e_i)$ items
with weight $w(e_i)/s(e_i)$ and size 1. The instance $I'$ has the same
capacity $C$.

Intuitively, we have sliced each item $e_i$ into $s(e_i)$ slices of size
1 and weight equal to the density of $e_i$. We call the items of $I'$
*slices*. Note that the total weight of the slices of $e_i$ is $w(e_i)$.
See @fig-knapsack-slices for an illustration.

:::: {figure label=fig-knapsack-slices}

(fig-knapsack-slices-1)=

::: {image width=50%} ./knapsack-slices-1.png

:::

(fig-knapsack-slices-2)=

::: {image width=50%} ./knapsack-slices-2.png

:::

@fig-knapsack-slices-1 illustrates a knapsack instance and
@fig-knapsack-slices-2 illustrates its fractional instance.

::::

First, we show that the optimal solution for $I'$ is an upper bound on
that for $I$. Define $\operatorname{OPT}(I)$ and
$\operatorname{OPT}(I')$ to be the weight of the max-weight feasible
solution for $I$ and $I'$, respectively. Observe that
$$\operatorname{OPT}(I') \geq \operatorname{OPT}(I).$$ This is because
for any solution $S$ for $I$, we can construct a feasible solution of
the same weight for $I'$ by pick every slice of every item in $S$.

Next, we characterize the optimal solution for $I'$. Suppose $i$ is the
largest integer such that $w(e_1) + \ldots + w(e_i) \leq C$. Observe
that $$A = \{e_1, \ldots, e_i\}.$$ If $i = n$, then $A$ chooses all
items and is optimal. In the remainder of the proof, we consider the
case $i < n$.

::: {image width=50%} ./knapsack.png

:::

The feasible sets of $I'$ is a [uniform matroid](#unif-matroid) since
every item has the same weight. Thus, [Marginal
Greedy](#alg-marginal-greedy) is [optimal](#thm-matroids). Observe that
Marginal Greedy chooses the first $C$ slices, in decreasing order of
weight. Since every slice of the same item has the same weight, Marginal
Greedy picks all the slices of $e_1, \ldots, e_i$ and only some of
$e_{i+1}$. As $A = \{e_1, \ldots, e_i\}$, we get that
$$w(A) + w(e_{i+1}) = \operatorname{OPT}(I') \geq \operatorname{OPT}(I).$$

With this upper bound on $\operatorname{OPT}(I)$, it is easy to complete
the analysis. Since $e^*$ is the heaviest item, we get that
$$w(A) + w(e^*) \geq \operatorname{OPT}(I),$$ and so either
$w(A) \geq \operatorname{OPT}(I)/2$ or
$w(e^*) \geq \operatorname{OPT}(I)/2$, as desired.

::::::

## Set Cover

Recall that in the [Coverage Maximization Problem](#def-coverage-max),
the goal is pick at most $k$ sets that cover as many elements as
possible. The Set Cover Problem is the minimization version, where the
objective and the constraints are swapped.

::: {prf:definition label=prob-set-cover} Set Cover

An instance of the *Set Cover Problem* consists of a set system with a
universe with $n$ elements and $m$ sets $S_1, \ldots, S_m$. The goal is
to find $I \subseteq [m]$ with $\operatorname{cov}(I) \geq n$ [^2] and
minimizes $|I|$. In other words, we want to cover every element using as
few sets as possible.

:::

The Set Cover Problem is an important optimization problem with a huge
range of applications. Whenever you have a problem where you want to
satisfy requirements using few resources, you can probably cast it as
Set Cover.

There is a natural greedy algorithm for this problem: while not every
element is covered, pick the set that covers as many uncovered elements
as possible.

::: {prf:definition label=alg-unweighted-greedy} Greedy Set Cover

- Initialize $I \leftarrow \emptyset$
- ****while**** $\operatorname{cove}(I) < n$ ****do****
  - Find $j^* \notin I$ that maximizes
    $\operatorname{cov}(I \cup \{j^*\}) - \operatorname{cov}(I)$
  - Add $j^*$ to $I$
- ****end for****
- ****return**** $I$

:::

Note that this algorithm is the same as the [greedy
algorithm](#alg-coverage-greedy) for Coverage Maximization, except for
the stopping condition: this one stops when every element is covered,
but the other one stops when $k$ sets have been chosen.

::: {prf:theorem label=thm-set-cover}

[Greedy Set Cover](#alg-unweighted-greedy) is a $(1 + \ln n)$
approximation for the Set Cover Problem.

:::

::: {prf:proof enumerated=false}

Let $I^*$ be the optimal solution and suppose $k = |I^*|$.

We show that after every $k$ iterations, the number of uncovered
elements shrink by at least a factor of $1/e$. Since
$\operatorname{cov}(I) = n$, and the first $k$ sets are exactly the ones
chosen by the [Coverage Maximization Greedy](#alg-coverage-greedy)
algorithm, and the algorithm is [guaranteed](#thm-coverage-max) to find
$k$ sets whose coverage is at least $(1-1/e)$ fraction of the maximum
coverage using $k$ sets, we get that after picking $k$ sets, the
fraction of uncovered elements is at most $1/e$.

We can generalize the above argument. Suppose that $U_i$ is the set of
uncovered elements after $ik$ iterations. Since $I^*$ can cover all of
$U_i$ using $k$ sets, the $k$ sets chosen in iterations $ik+1$ up to
$(i+1)k$ cover at least $(1-1/e)$ fraction of $U_i$ and thus,
$$|U_{i+1}| \leq \frac{1}{e}|U_i| \leq \left(\frac{1}{e}\right)^{i+1}n.$$

Since $|U_i|$ is an integer, we know that $|U_i| = 0$ when
$(1/e)^i < 1/n$. Therefore, the total number of sets chosen by greedy is
at most $(\ln n + 1)k$.

:::

The natural follow up question is: can we do better? It turns out that
not only is computing the exact solution for Set Cover NP-hard, but any
meaningful improvement on Greedy's approximation ratio is also NP-hard!

What is a "meaningful improvement"? Typically, for minimization
problems, given two approximation ratios $r_1, r_2$, we think of $r_1$
as a meaningful improvement on $r_2$ if $r_1 = o(r_2)$. In the case of
Set Cover, a meaningful improvement on Greedy is an approximation ratio
of $o(\ln n)$. In other words, a constant-factor improvement (e.g. to
$(\ln n)/2$), is usually not considered interesting.

Remarkably, it has been shown that not only is $o(\ln n)$ NP-hard, but
even improving the constant is NP-hard! The following result is one of
the landmark results in the theory of approximation algorithms.

::: {prf:theorem label=thm-set-cover-hardness}

For every $\epsilon > 0$, it is NP-hard to approximate Set Cover to
within $(1-\epsilon)\ln n$.

:::

The proof is out of the scope of this subject, but we will use this
result to show inapproximability of other problems.

The arguments in the proof of @thm-set-cover can be used to show that
for every $\alpha > 0$, a $(1-1/e+α)$-approximation for Coverage
Maximization implies a $(1-\epsilon)\ln n$ approximation for Set Cover
for some $\epsilon > 0$. As a consequence, it is not possible to improve
upon the approximation ratio for Coverage Maximization.

::: {prf:theorem}

For every $\alpha > 0$, it is NP-hard to approximate Coverage
Maximization to within $(1-1/e+\alpha)$.

:::

In @sec-tut4, we will make the above arguments precise.

[^1]: Recall that the approximation ratio of an algorithm is the
    worst-case approximation ratio of the algorithm over every instance
    of the problem.

[^2]: Recall that $\operatorname{cov}(I)$ is the number of elements
    covered by the sets indexed by $I$.
