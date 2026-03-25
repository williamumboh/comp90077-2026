(sec-submodular-max)=

# Submodular Maximization

Previously, we considered optimization problems with
[additive](#def-additive) objective functions. In this section, we
consider a more general class of functions called submodular functions
which appear in many practical applications. While these problems are
NP-hard, it turns out that greedy yields the best-possible approximation
algorithms.

## Coverage Maximization

We begin by considering the coverage maximization problem. First, we
need to define a set system.

::: {prf:definition label=def-set-system} Set System

A *set system* consists of:

- a *universe* $U$ of $n$ elements
- a collection of $m$ sets $S_1, \ldots, S_m \subseteq U$.

:::

For a subset $I \subseteq [m]$, define their *coverage*
$\operatorname{cov}(I) = |\bigcup_{j \in I} S_j|$.

::: {prf:definition label=def-coverage-max} Coverage Maximization

An instance of the *Coverage Maximization Problem* consists of a set
system with a universe with $n$ elements and $m$ sets
$S_1, \ldots, S_m$, and a bound $k$. The goal is to find
$I \subseteq [m]$ with $|I|\leq k$ and maximizes
$\operatorname{cov}(I)$. In other words, we want to find $k$ sets that
cover as many elements as possible.

:::

The Coverage Maximization Problem appears in many applications. For
example, a telecommunications company may wish to install $k$ radio
towers from among $m$ different possible locations. Each location covers
a different subset of $n$ customers. The goal is maximize the number of
customers covered by at least one tower.

There is a natural greedy algorithm that iteratively picks the set that
covers the most elements that were previously uncovered.

::: {prf:algorithm label=alg-coverage-greedy}

- Initialize $I \leftarrow \emptyset$
- ****while**** $|I| < k$ ****do****
  - Find $j^* \notin I$ that maximizes
    $\operatorname{cov}(I \cup \{j^*\}) - \operatorname{cov}(I)$
  - Add $j^*$ to $I$
- ****end for****
- ****return**** $I$

:::

::: {prf:theorem label=thm-coverage-max}

The [greedy algorithm](#alg-coverage-greedy) is a
$(1 - 1/e)$-approximation algorithm for the [Coverage Maximization
Problem](#def-coverage-max).

:::

::: {prf:proof enumerated=false}

Let $I^*$ be an optimal solution and let $C^*$ be the elements covered
by $I^*$. Let $I_\ell$ be the set $I$ at the end of the $ℓ$-th
iteration. Let $C_\ell$ be the elements covered by $I$ at the end of the
$ℓ$-th iteration. We first show that after each iteration, the gap
between $|C^*|$ and $|C_\ell|$ decreases by a factor of $(1-1/k)$, i.e.
$$|C^*| - |C_\ell| \leq \left(1 - \frac{1}{k}\right) (|C^*| - |C_{\ell-1}|).$$
Consider some iteration $\ell \leq k$. By definition, every element of
$C^* \setminus C_{\ell-1}$ is contained in $S_j \setminus C_{\ell-1}$
for some $j \in I^*$, i.e.
$$C^* \setminus C_{\ell-1} \subseteq \bigcup_{j \in I^*} (S_j \setminus C_{\ell-1})$$
and so

$$|C^* \setminus C_{\ell-1}| \leq \sum_{j \in I^*} |S_j \setminus C_{\ell-1}|.$$
Since $|I^*| = k$, there is some $j \in I^*$ such that
$$|S_j \setminus C_{\ell-1}| \geq \frac{|C^* \setminus C_{\ell-1}|}{k} \geq \frac{|C^*| - |C_{\ell-1}|}{k}$$
Adding the set $S_j$ increases the algorithm's coverage by
$|S_j \setminus C_{\ell-1}|$. So by definition of the greedy choice, the
increase in the algorithm's coverage is
$$|C_\ell| - |C_{\ell-1}| \geq \frac{|C^*| - |C_{\ell-1}|}{k}.$$ Thus,
we have
$$|C^*| - |C_\ell| &\leq |C^*| - |C_{\ell-1}| - \frac{|C^*| - |C_{\ell-1}|}{k} \\ &= \left(1 - \frac{1}{k}\right) (|C^*| - |C_{\ell-1}|),$$
as desired.

With the above and the fact that $C_0 = \emptyset$, we get that
$$|C^*| - |C_k| &\leq \left(1-\frac{1}{k}\right)^k (|C^*|-|C_0|) \\ &=\left(1-\frac{1}{k}\right)^k|C^*|,$$
and so
$$|C_k| \geq \left(1 - \left(1 - \frac{1}{k}\right)^k\right)|C^*| \geq \left(1 - \frac{1}{e}\right)|C^*|$$
where the last inequality follows from the fact[^1] that
$1 + x \leq e^x$ for every $x$.

Since $C_k$ is exactly the elements covered by the algorithm, we get the
desired approximation ratio.

:::

## Submodular Maximization

Just as we did when we generalized from the Minimum Spanning Tree
Problem to the Max-Weight Basis Problem, we can ask what are the
essential properties of the Coverage Maximization Problem and whether
the arguments can be generalized. It turns out that the main thing in
the analysis that we need is a property called submodularity.

::: {prf:definition label=def-submodular} Submodular Function

A set function $g : 2^E \rightarrow \mathbb{R}$ is *submodular* if it
has the decreasing marginals property: for every two sets $S, T$ and
element $e$ such that $S \subseteq T$ and $e \notin T$, we have that the
marginal increase of $g$ when $e$ is added to $S$ is at least as large
as the marginal increase of $g$ when $e$ is added to a set $T$
containing $S$: $$g(S \cup \{e\}) - g(S) \geq g(T \cup \{e\}) - g(T).$$

An equivalent definition is that for every two sets $S,T$, we have
$$g(S) + g(T) \geq g(S \cap T) + g(S \cup T).$$

:::

::: {prf:definition} Monotone Set Functions

A set function $g$ is *monotone* if for every sets $S, T$ such that
$S \subseteq T$, we have $g(S) \leq g(T)$.

:::

::: {prf:theorem} Coverage is Submodular

For every $n,m$ and every set system with $n$ elements and $m$ sets
$S_1, \ldots, S_m$, the coverage function $\operatorname{cov}(\cdot)$ is
submodular.

:::

::: {prf:proof enumerated=false label=prf-cov-max}

For every $A \subseteq [m]$, define $C(A) = \bigcup_{j \in A} S_j$, i.e.
the elements covered by the sets indexed by $A$. Note that
$\operatorname{cov}(A) = |C(A)|$ and
$$\operatorname{cov}(A \cup \{j\}) - \operatorname{cov}(A) = |C(A) \cup S_j| - |C(A)| = |S_j \setminus C(A)|.$$

Suppose we have $X, Y \subseteq [m]$ such that $X \subseteq Y$ and let
$j \notin Y$. Observe that $C(X)$ is contained in $C(Y)$ and so
$S_j \setminus C(Y)$ is contained in $S_j \setminus C(X)$. Putting all
these together, we get
$$\operatorname{cov}(X \cup \{j\}) - \operatorname{cov}(X)
&= |S_j \setminus C(X)|\\
&\geq |S_j \setminus C(Y)|
= \operatorname{cov}(Y \cup \{j\}) - \operatorname{cov}(Y).$$

:::

As many real world problems exhibit the decreasing marginals property,
submodular functions are widely applicable. For example,

- when the cost of purchasing is cheaper per unit when buying in bulk,
  and thus can be modeled as $g(S) = f(|S|)$ where
  $f : \mathbb{R} \rightarrow \mathbb{R}$ is a concave function;
- the amount of utility a consumer obtains from a set of substitute
  goods[^2] has decreasing marginals.

::: {prf:definition label=prob-submodular-max} Submodular Maximization

An instance of the *Cardinality-Constrained Monotone Submodular
Maximization Problem* consists of a ground set $E$ of $n$ elements, a
non-negative monotone submodular function
$g :2^E \rightarrow \mathbb{R}$, and a bound $k$. The goal is find a
subset $I$ with $|I| \leq k$ and maximizes $g(I)$.

:::

::: {prf:algorithm label=alg-submodular-greedy}

- Initialize $I \leftarrow \emptyset$
- ****while**** $|I| < k$ ****do****
  - Find $e \notin I$ that maximizes $g(I \cup \{e\}) - g(I)$
  - Add $e$ to $I$
- ****end for****
- ****return**** $I$

:::

::: {prf:theorem label=thm-submod-max}

The [greedy algorithm](#alg-submodular-greedy) is a
$(1 - 1/e)$-approximation algorithm for the [Cardinality-Constrained
Submodular Maximization Problem](#prob-submodular-max).

:::

::: {prf:proof enumerated=false label=prf-submod-max}

Let $I^* = \{e^*_1, \ldots, e^*_k\}$ be an optimal solution. Let
$I_\ell$ be the set $I$ at the end of the $ℓ$-th iteration. As in the
proof of @thm-coverage-max, we first show that the gap between $g(I^*)$
and $g(I_\ell)$ decreases by a factor of $(1-1/k)$ in each iteration,
i.e.
$$g(I^*) - g(I_\ell) \leq \left(1 - \frac{1}{k}\right) (g(I^*) - g(I_{\ell-1})),$$
by showing that there exists an element $e \in I^* \setminus I_{\ell-1}$
such that
$$g(I_{\ell-1} \cup \{e\}) - g(I_{\ell-1}) \geq \frac{g(I^*) - g(I_{\ell-1})}{k}.$$

To that end, we take a closer look at $g(I^*) - g(I_{\ell-1})$. By
monotonicity, we have
$$g(I^*) - g(I_{\ell-1}) \leq g(I^* \cup I_{\ell-1}) - g(I_{\ell-1}).$$
Then, we express the RHS of the above inequality as a sum of differences
as follows. Define $T_j = \{e^*_1,\ldots,e^*_j\} \cup I_{\ell-1}$ and
$T_0 = I_{\ell-1}$. Then, we have
$$g(I^* \cup I_{\ell-1}) - g(I_{\ell-1}) \leq \sum_{\ell = 1}^k g(T_\ell) - g(T_{\ell-1}).$$

By averaging, there exists $i$ such that
$$\frac{g(I^* \cup I_{\ell-1}) - g(I_{\ell-1})}{k} \leq g(T_{i}) - g(T_{i-1}).$$
Since $T_{i} = T_{i-1} \cup \{e^*_i\}$, we get
$$g(T_{i}) - g(T_{i-1}) = g(T_{i-1} \cup \{e^*_i\}) - g(T_{i-1}).$$
Moreover, $T_{i-1}$ contains $T_0 = I_{\ell-1}$, so by
[submodularity](#def-submodular), we have
$$g(T_{i-1} \cup \{e^*_i\}) - g(T_{i-1}) \leq g(I_{\ell-1} \cup \{e^*_i\}) - g(I_{\ell-1}).$$

Putting all of the above together, we get that
$$g(I_{\ell-1} \cup \{e^*_i\}) - g(I_{\ell-1}) \geq \frac{g(I^*) - g(I_{\ell-1})}{k},$$
as desired.

The rest of the proof is similar as in the [proof](#prf-cov-max) of
@thm-coverage-max. We have
$$g(I^*) - g(I_k) \leq \left(1-\frac{1}{k}\right)^k g(I^*)$$ and so
$$g(I_k) \geq \left(1 -\left(1-\frac{1}{k}\right)^k\right) g(I^*) \geq \left(1 - \frac{1}{e}\right)g(I^*).$$

:::

[^1]: This fact can be obtained from the Taylor expansion of $e^x$.

[^2]: Substitute goods are goods that can be used in place of another,
    e.g. cars.
