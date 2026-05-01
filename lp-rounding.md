(sec-lp-rounding)=

# LP Rounding

Linear Programming has been incredibly successful in the design and
analysis of approximation algorithms. In the textbooks [The Design of
Approximation Algorithms](http://www.designofapproxalgs.com/book.pdf)
and [Approximation Algorithms](https://ics.uci.edu/~vazirani/book.pdf),
linear programming play a central role. A key reason is that fractional
relaxations of problems let us obtain a bound on the optimal solution

There are several techniques based on linear programming. In this
lecture, we continue developing the LP rounding technique introduced in
@sec-vc-lp.

## General Framework for LP Rounding

LP rounding algorithms follow the same general template. Consider a
[combinatorial optimization problem](#prob-comb) $\Pi$ with an [additive
objective function](#def-additive), i.e. each instance is of the form
$$\text{minimize} \quad & \sum_{i \in S} w_i\\
\text{subject to} \quad & S \in \mathcal{F}$$
where $\mathcal{F}$ is the set of feasible solutions.

The first step is to write an integer program IP that is *equivalent* to
$\Pi$ in the sense that feasible solutions to the integer program are in
1-1 correspondence with feasible solutions to $\Pi$ (we will formally
define this later). The IP is also sometimes called the *integer
programming formulation* of $\Pi$. Typically, one does this by
introducing IP variables $x_i$ that are *indicator variables*, i.e.
$x_i = 1$ if we want to include $i$ in our solution $S$ and $x_i = 0$
otherwise. Then, one introduces linear inequalities $Bx \geq c$ such
that $S \in \mathcal{F}$ if and only if $Bx \geq c$.

::: {important} Equivalence of IP and $\Pi$

Formally, the integer program IP is equivalent to $\Pi$ if and only if
the following two conditions hold:

- for every $S \in \mathcal{F}$, there exists $x \in \{0,1\}^n$ such
  that $Bx \leq c$ and $\sum_{i \in S} w_i = \sum_i w_ix_i$; and
- for every $x \in \{0,1\}^n$ such that $Bx \leq c$, there exists
  $S \in \mathcal{F}$ such that $\sum_{i \in S} w_i = \sum_i w_ix_i$.

Typically, there is an obvious IP that is equivalent—called the
*natural* IP—and we will skip the proof in such cases.

:::

The second step is to relax the integer program into a linear program
LP, and solve the LP. Here, one needs to argue that the LP can indeed be
solved efficiently. It is often sufficient to show that the number of
variables $n$ and the number of constraints $m$ are polynomial. In the
next lecture, we will see how to solve the LP efficiently even if $m$ is
exponential.

The key fact about the linear relaxation we will use is the following
lemma, which follows from the fact that every feasible solution to the
LP is also feasible to the IP, and thus the optimal LP solution can only
be better than the optimal IP solution.

::: {prf:lemma label=lem-relax} LP vs IP

For every integer program IP and its linear relaxation LP, we have

- for minimization problems,
  $\operatorname{OPT-LP} \leq \operatorname{OPT-IP}$
- for maximization problems,
  $\operatorname{OPT-LP} \geq \operatorname{OPT-IP}$

:::

The third step is to show how an optimal LP solution $x^*$ can be
rounded into an integral solution $X$ (i.e. solution to the IP) such
that $$\sum_i w_iX_i \leq \alpha \sum_i w_ix^*_i.$$

Finally, we output as solution to $\Pi$ the subset $S$ where $i \in S$
if and only if $X_i = 1$. To see why this is an $α$-approximation for
$\Pi$, let $X^*$ be optimal solution to the IP and $S^*$ be an optimal
solution to $\Pi$. Then, $$\sum_{i \in S} w_i
&= \sum_i w_iX_i \\
&\leq \alpha \sum_i w_ix^*_i \\
&\leq \sum_i w_i X^*_i\\
&= \sum_{i \in S^*} w_i.$$ The second last line used @lem-relax and the
last line used the equivalence of the IP and $\Pi$.

Typically, the way we find $X$ such that
$$\sum_i w_iX_i \leq \alpha \sum_i w_ix^*_i.$$ is by finding $X$ such
that for every $i$, we have $X_i \leq \alpha x^*_i$. This is what we did
when we rounded the [vertex cover LP](#sec-vc-lp). This will be the
strategy that we use for this lecture.

The above discussion is summarized in the following lemma.

::: {prf:lemma label=lem-lp-rounding}

Let $x^*$ be an optimal solution to the LP relaxation of an IP for a
combinatorial optimization problem $\Pi$. If $X$ is an IP solution such
that $X_i \leq \alpha x^*_i$ for every $i$, then the set $S$ where
$i \in S$ if and only if $X_i = 1$ is an $α$-approximation for $\Pi$.

:::

(sec-weighted-set-cover)=

## Weighted Set Cover

We consider the Weighted Set Cover problem.

::: {prf:definition label=prob-wt-set-cover} Weighted Set Cover

An instance of the *Weighted Set Cover Problem* consists of a set system
with a universe with $n$ elements and $m$ sets $S_1, \ldots, S_m$. Each
set $S_i$ has a weight $w_i$. The goal is to find $I \subseteq [m]$ with
$\operatorname{cov}(I) \geq n$ [^1] and minimizes $\sum_{i \in I} w_i$.

:::

The natural IP for Weighted Set Cover is as follows:
$$\text{minimize} \quad & \sum_i w_ix_i\\
\text{subject to} \quad & \sum_{i: S_i \ni e} x_i \geq 1 \quad  && \forall e \in U\\
& x_i \in \{0,1\} \quad  && \forall i \in [m]$$

There are two LP-rounding algorithms for Set Cover. We begin with the
simpler of the two.

### Deterministic LP Rounding

The deterministic LP rounding algorithm is a straightforward
generalization of the one for vertex cover.

::: {prf:definition label=def-freq} Frequency

The *frequency* $f$ of a set system is
$\max_{e \in U} |\{i : S_i \ni e\}|$, i.e. the maximum number of sets
that any element belongs to.

:::

Note that Vertex Cover is Set Cover with frequency 2 since every edge is
covered by exactly two vertices.

Let $x^*$ be an optimal LP solution. The key idea is that for every
element $e \in U$, the LHS of the inequality
$$\sum_{i: S_i \ni e} x_i \geq 1$$ is a sum of at most $f$ terms and
thus, there exists a set $S_i \ni e$ with $x^*_i \geq 1/f$.

Therefore, the integral solution $X$ where $X_i = 1$ if $x^*_i \geq 1/f$
and $X_i = 0$ if $x^*_i < 1/f$ is a feasible IP solution. We also have
that $X_i \leq f x^*_i$ for every $i$ and so by @lem-lp-rounding, we get
an $f$-approximation.

::: {prf:theorem label=thm-set-cover-f}

There is a $f$-approximation for Weighted Set Cover.

:::

## Weighted Set Cover - Randomized Approximation

An $f$-approximation is undesirable in settings where $f$ can be as
large as $m$, the number of sets.

We now give a randomized algorithm that gives an
$O(log n)$-approximation. The running time of the algorithm is
polynomial but its solution is probabilistic and the guarantee is that
the expected cost of its solution is at most $O(\log n)$ of the optimal.

### First Idea: Random Sampling

Let $x^*$ be an optimal LP solution. A natural first idea is to treat
the variables $x^*_i$ as probabilities and to sample each set
accordingly.

::: {prf:algorithm label=alg-set-cover-1}

- Initialize $X_i = 0$ for every $i \in [m]$
- ****foreach**** $i \in [m]$ ****do****
  - Independently set $X_i = 1$ with probability $x^*_i$
- ****endfor****
- ****return**** $X$

:::

The expected cost of the integer solution $X$ is as good as we can hope
for. By linearity of expectation, we have
$$E\left[\sum_i w_iX_i\right] = \sum_i w_iE[X_i] = \sum_i w_ix^*_i.$$
However, $X$ is not necessarily a feasible solution, i.e. it does not
necessarily cover all elements.

### Probability Amplification + Alteration

To fix this, we will use two ideas. The first is called *probability
amplification*: sample each set $S_i$ with probability $\alpha x^*_i$
for some factor $\alpha > 1$ to be determined later on. This ensures
that every element is covered with high probability. The second idea is
called *alteration* where we alter the probabilistic solution; in this
case, we alter $X$ to ensure every element is covered.

For each element $e$, let $S_e$ be the minimum-weight set that contains
$e$ and let $w_e$ be its weight.

::: {prf:algorithm label=alg-set-cover-2}

- Initialize $X_i = 0$ for every $i \in [m]$
- ****foreach**** $i \in [m]$ ****do****
  - Independently set $X_i = 1$ with probability $\alpha x^*_i$
- ****endfor****
- ****foreach**** $e \in U$ ****do****
  - If $e$ not covered, set $X_i = 1$ where $S_i = S_e$
- ****endfor****
- ****return**** $X$

:::

We will set $\alpha$ so that the probability that $X$ is feasible, i.e.
every element is covered, is sufficiently high. We now bound the
probability that a fixed element is not covered.

::: {prf:lemma label=lem-set-cover-prob}

For every element $e$, we have
$$\Pr[\text{$e$ not covered}] \leq \exp(-\alpha).$$

:::

::: {prf:proof enumerated=false}

Fix an element $e \in U$. It is not covered if and only if none of the
sets containing it is chosen. Thus, we have
$$\Pr[\text{$e$ not covered}]
&= \Pr\left[\bigwedge_{i : S_i \ni e} \{X_i = 0\}\right] \\
&= \prod_{i : S_i \ni e} \Pr[X_i = 0] \\
&\leq \prod_{i : S_i \ni e} (1 - \alpha x^*_i).$$
Using the fact that $(1 - z) \leq \exp(-z)$, we have
$$\prod_{i : S_i \ni e} (1 - \alpha x^*_i)
&\leq \prod_{i : S_i \ni e} \exp(-\alpha x^*_i) \\
&= \exp(-\alpha \sum_{i : S_i \ni e} x^*_i)\\
&\leq \exp(-\alpha),$$
where the last line follows from the linear inequalities of the LP.

:::

::: {prf:lemma label=lem-set-cover-cost}

The expected cost of $X$ is
$$E\left[\sum_i w_iX_i\right] \leq (\alpha + n\exp^{-\alpha}) \sum_i w_i x^*_i.$$

:::

::: {prf:proof enumerated=false}

Call the first **for** loop of @alg-set-cover-2 the *sample phase* and
the second **for** loop the *backup phase*. The expected cost of the
sample phase is $\alpha \sum_i w_ix^*_i$.

To bound the cost due to the backup phase, first observe that for every
element $e$, we have $$w_e \leq \sum_i w_i x^*_i.$$ This is because
$\sum_{i : S_i \ni e} x^*_i \geq 1$ and so $$\sum_i w_i x^*_i
&\geq \sum_{i : S_i \ni e} w_i x^*_i \\
&\geq \min_{i : S_i \ni e} w_i = w_e.$$

By @lem-set-cover-prob, in the backup phase, each element $e$ needs to
be covered at cost $w_e \leq \sum_i w_ix^*_i$ with probability at most
$\exp(-\alpha)$. Summing over all $n$ elements, the expected cost of the
backup phase is at most $n\exp(-\alpha)\sum_i w_i x^*_i$.

:::

Thus, the approximation ratio is $\alpha + n\exp^{-\alpha}$. Setting
$\alpha = \ln n$ yields an approximation ratio of $(\ln n + 1)$.

::: {prf:theorem label=thm-wt-set-cover}

There is a randomized $(\ln n +1)$ approximation algorithm for Weighted
Set Cover.

:::

## Integrality Gaps

Given an LP rounding algorithm, a natural question to ask is whether our
rounding scheme is as good as it can get. Note that we are not asking
for a better approximation algorithm that uses a different LP or a
non-LP based algorithm for the same problem.

::: {prf:definition label=def-int-gap}

The *integrality gap* of an LP is the worst-case
$\operatorname{OPT-IP}/\operatorname{OPT-LP}$ over all instances.

:::

The integrality gap of an LP is a lower bound on the best approximation
ratio that can be obtained by rounding the LP. The below results show
that we gotten as much mileage as we can out of the Set Cover and Vertex
Cover LPs.

::: {prf:theorem}

The integrality gap of the Set Cover LP in @sec-weighted-set-cover is
$\Omega(\log n)$, even for unweighted Set Cover.

:::

::: {prf:theorem}

The integrality gap of the Vertex Cover LP in @sec-vc-lp is at least
$2(1-1/n)$, even for unweighted Vertex Cover.

:::

::: {prf:proof enumerated=false}

In @ex-6-2 of @sec-tut6, we [showed](#sol-6-2) that the integrality gap
is at least $4/3$. We now show that we can get an integrality gap of
$2(1-1/n)$ using a graph with $n$ vertices.

Intuitively, to construct an integrality gap instance, we want a
fractional solution that is as fractional as possible. In the case of
the vertex cover LP, this corresponds to setting $x_v = 1/2$ for every
vertex $v$. Note that the fractional solution has total cost $n/2$ and
is feasible regardless of the edge set. Now that we have fixed the
fractional solution, we want to add edges to make the integral vertex
cover as large as possible. Intuitively, the more edges the larger the
integral vertex cover should be. Indeed, if the graph is complete (i.e.
has all ${n \choose 2}$ edges), then the smallest integral vertex cover
is to choose $n-1$ vertices; if it misses 2 vertices $u$ and $v$, then
it does not cover the edge $(u,v)$.

Thus, the integrality gap is at least $n-1/(n/2) = 2(1- 1/n)$.

:::

## Min $(s,t)$-Cut

Graph Partitioning is a class of problems in which we are given a graph
$G = (V,E)$ with edge costs and we want to partition the vertex set $V$
into parts $V_1, \ldots, V_k$ to minimize the total cost of edges that
cross between parts. These problems occur in applications such as
clustering. They are also interesting as they are duals to max flow
problems, used as subroutines for divide-and-conquer algorithms for
graph optimization problems, and have deep connections to metric
embeddings and graph theory.

The most basic graph partitioning problem is the Min $(s,t)$-Cut
problem. Given a subset $S$ of vertices in a graph $G = (V,E)$, define
$\delta(S) = \{(u,v) \in E: |\{u,v\} \cap S| =1\}$, the set of edges
with exactly one endpoint in $S$ and the other outside of $S$. We also
call $\delta(S)$ the *cut edges* of $S$.

::: {prf:definition label=prob-st-cut}

Given a graph $G = (V,E)$ with edge costs $c_e$ and two vertices
$s,t \in V$, the goal is to partition $V$ into two pieces $S,T$ such
that $s \in S$ and $t \in T$ while minimizing the cost of cut edges
$c(S) = \sum_{e \in \delta(S)} c_e$.

:::

(sec-cut-IP)=

### Integer Program Formulation

How do we formulate this as an integer program? There are several ways
to do this, each arising from a different way of formulating the
decisions that the algorithm has to make. The idea behind the
formulation we use is that the pieces $S,T$ is completely determined if
we know for each pair $u,v \in V$, whether $u$ and $v$ are in the same
piece: the set $S$ is simply the vertices that are in the same piece as
$s$, and $T$ those that are in the same piece as $t$. For brevity, we
say that $u$ and $v$ are *cut* if they are in different pieces.

Our integer program has a variable $d_{uv}$ for each $u,v$ where
$$d_{uv} =
\begin{cases}1 & \text{if $u$ and $v$ are cut} \\
0 & \text{$u$ and $v$ are not cut}
\end{cases}$$

The objective function is clear: $\sum_{(u,v) \in E} d_{uv} c_{uv}$.
Now, we need to add linear inequalities. The first is that $s$ and $t$
have to be cut: $d_{st} = 1$. We still need more inequalities since
otherwise, the solution with $d_{st} = 1$ and $d_{uv} = 0$ whenever at
least one of $u,v$ is not $s$ or $t$ is a feasible solution to the
integer program and has cost 0 if $(s,t) \notin E$.

Observe that since $s$ and $t$ are cut, then for every other vertex $v$,
we have that either $s$ and $v$ are cut or $t$ and $v$ are cut.
Generalizing this observation, we see that if vertices $u$ and $v$ are
cut, then for every other vertex $w$, either $u$ and $w$ are cut or $v$
and $w$ are cut. This gives the triangle inequality
$d_{uv} \leq d_{uw} + d_{vw}$.

In summary, we get the following integer program.
$$\text{minimize} \quad & \sum_{(u,v) \in E} d_{uv}c_{uv}\\
\text{subject to} \quad
& d_{st} = 1 \quad  && \\
& d_{uv} \leq d_{uw} + d_{vw} && \forall u,v,w \in V\\
& d_{uv} \in \{0,1\} \quad  && \forall u,v \in V$$

### Rounding

Since the variables $d_{uv}$ satisfy the triangle inequality, we can
view a feasible LP solution $d$ as a distance function.[^2]

Let $d^*$ be an optimal fractional solution. The idea for the rounding
is to pick a random radius $\Theta \in [0,1)$ and to set $S$ to be the
set of vertices $v$ with $d^*(s,v) \leq \Theta$. In other words, $S$ is
the ball of radius $\Theta$ centered at $s$.

::: {prf:algorithm label=alg-st-cut}

- Pick $\Theta \in [0,1)$ uniformly at random
- Set $S = \{v : d^*(s,v) \leq \Theta\}$
- Set $T = \{v : d^*(s,v) > \Theta\}$
- ****return**** $(S,T)$

:::

Since $d^*_{st} = 1 > \Theta$, we are guaranteed that $S$ will always
contain $s$ and never contain $t$. Thus, $(S,T)$ is always a feasible
solution.

::: {prf:lemma label=lem-cut-prob}

The expected cost of the cut edges is
$$E\left[\sum_{e \in \delta(S)} c_e \right]
\leq \sum_{(u,v) \in E} d^*_{uv}c_{uv}.$$

:::

::: {prf:proof enumerated=false}

Let us now analyze the probability that an edge $(u,v)$ is in
$\delta(S)$. Suppose that $d(s,v) \leq d(s,u)$. Then, the edge $(u,v)$
is in $\delta(S)$ if and only if $$d^*(s,v) \leq \Theta < d^*(s,u).$$
Since $\Theta$ is chosen uniformly at random from the interval $[0,1)$,
we have
$$\Pr[d^*(s,v) \leq \Theta < d^*(s,u)]
&= d^*(s,u)-d^*(s,v) \\
&\leq d^*(u,v),$$
where the last step follows from the triangle inequality.

:::

We have now shown that the expected cost of the solution is at most that
of the optimal LP solution and so is optimal.

### But wait, there's more!

We can actually easily derandomize @alg-st-cut using the following
lemma.

::: {prf:lemma label=lem-min-st-cut-det}

For each $1 \leq i \leq n$, let $v_i$ be the $i$-th nearest vertex to
$s$ and $S_i$ be the set of $i$ nearest vertices to $s$, according to
distances $d^*$.[^3] Then $S_i$ is an optimal $(s,t)$-cut for every
$1 \leq i \leq n$.

:::

::: {prf:proof enumerated=false}

Since the [randomized $(s,t)$-cut algorithm](#alg-st-cut) chooses $S$
where $S$ is the set of vertices within some radius $\Theta$ of $s$, we
get that $S = S_i$ for some $i$. Let $p_i = \Pr[S = S_i]$.

Let $C^*$ be the cost of the minimum $(s,t)$-cut. By @lem-cut-prob, we
have
$$\sum_i p_i c(S_i) = C^*.$$
By optimality, we have that $c(S_i) \geq C^*$ for every
$1 \leq i \leq n$. Therefore, we have that $c(S_i) \geq C^*$ for every
$1 \leq i \leq n$.[^4]

:::

Thus, we can easily derandomize @alg-st-cut by picking
$S = \{v: d(s,v) = 0\}$.

::: {prf:theorem label=thm-min-st-cut-det}

There is a deterministic exact algorithm for Min $(s,t)$-Cut.

:::

[^1]: Recall that $\operatorname{cov}(I)$ is the number of elements
    covered by the sets indexed by $I$.

[^2]: For those familiar with metric spaces, you can interpret the LP as
    an embedding the vertices $V$ into a metric space.

[^3]: Here, $s = v_1$.

[^4]: Intuitively, if we take the average of a set of numbers, one of
    the numbers is strictly more than the average if and only if some
    other number in the set is strictly less than the average.
