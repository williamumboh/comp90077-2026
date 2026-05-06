(sec-tut8)=

# Tutorial 8

## Alternative IP for Min $(s,t)$-cut

The first two questions concern an alternative integer program for Min
$(s,t)$-Cut.

::: {exercise label=ex-8-1}

Another reasonable integer programming formulation for [Min
$(s,t)$-Cut](#prob-st-cut) is based on choosing a minimum cost set of
edges $F$ to remove so that $s$ and $t$ are disconnected. The integer
program has a variable $x_e$ for each edge $e \in E$ indicating whether
$e \in F$. To ensure that $s$ and $t$ are disconnected, we need to
remove at least one edge from each $(s,t)$-path. Let $\mathcal{P}_{s,t}$
be the set of $(s,t)$-paths $P$.

The integer program $\operatorname{Path-IP}$ is as follows.
$$\text{minimize} \quad & \sum_{e \in E} x_ec_e\\
\text{subject to} \quad & \sum_{e \in P} x_e \geq 1 \quad  && \forall P \in \mathcal{P}_{s,t}\\
& x_e \in \{0,1\} \quad  && \forall e \in E$$
Its LP relaxation, where the integrality constraints are relaxed to
nonnegativity constraints, i.e. $x_e \geq 0$, is called
$\operatorname{Path-LP}$.

Show that $\operatorname{Path-LP}$ is equivalent to
$\operatorname{Metric-LP}$, the LP relaxation of
[$\operatorname{Metric-IP}$](#metric-IP) integer program from lecture.
In other words:

1.  for every feasible solution $x$ to $\operatorname{Path-LP}$, there
    is a feasible solution $d$ to $\operatorname{Metric-LP}$ with cost
    $\sum_{(u,v) \in E} d_{uv} c_uv \leq \sum_{e \in E} x_ec_e$ and
2.  for every feasible solution $d$ to $\operatorname{Metric-LP}$, there
    is a feasible solution $x$ to $\operatorname{Path-LP}$ with cost
    $\sum_{e \in E} x_ec_e \leq \sum_{(u,v) \in E} d_{uv} c_uv$.

:::

::: {solution class=dropdown} ex-8-1

Let $x$ be a feasible solution to $\operatorname{Path-LP}$. Consider the
solution $d$ to $\operatorname{Metric-LP}$ where for every $u,v \in V$,
we define $d_{uv}$ to be the shortest-path distance between $u$ and $v$
in the graph $G$ with edge lengths $x_e$.

We first argue that $d$ is a feasible solution. We have
$d_{uv} \leq d_{uw} + d_{wv}$ since the shortest path between $u$ and
$v$ cannot be longer than going from $u$ to $w$ to $v$. The
nonnegativity constraints are satisfied since the edge lengths $x_e$ are
also nonnegative due to the nonnegativity constraints of
$\operatorname{Metric-LP}$. The constraints of $\operatorname{Path-LP}$
ensures that the shortest path between $s$ and $t$ is at least $1$ and
so $d_{st} \geq 1$. Define a new solution $d'$ where
$d'_{uv} = d_{uv}/d_{st}$ for every $u,v \in V$. We have that
$d'_{st} = 1$ and $d'_{uv} \leq d_{uv}$ for every $u,v \in V$. Moreover,
$d'$ also satisfies the triangle inequalities and the nonnegative
constraints.

Finally, to see that the LP cost of $d'$ is at most that of $x$, observe
that for every $(u,v) \in E$, we have $d'_{uv} \leq d_{uv} \leq x_{uv}$
where the second inequality follows from the fact that the edge $(u,v)$
is a path between $u$ and $v$.

On the other hand, let $d$ be a feasible solution
$\operatorname{Metric-LP}$. Consider the solution $x$ to
$\operatorname{Path-LP}$ where for every $(u,v) \in E$, we define
$x_{uv} = d_{uv}$. The nonnegativity constraints are satisfied since $d$
is nonnegative. For every $P \in \mathcal{P}_{s,t}$, since $d$ satisfies
the triangle inequality and $d_{st}=1$, we have
$$\sum_{e \in P} x_e
&= \sum_{e \in P} d_e \\
&\geq d_{st} = 1.$$
By definition of $x$, its LP cost is the same as that of $d$.

:::

::: {exercise label=ex-8-2}

Assume you are given an optimal solution $x^*$ to the above LP[^1]. Show
that you can round it to an integral solution $X$ whose cost is at most
that of $x^*$.

:::

::: {hint class=dropdown}

Interpret the variables $x_e$ as edge lengths.

:::

::: {hint class=dropdown}

Pick a ball with random radius centered at $s$.

:::

::: {solution class=dropdown} ex-8-2

Define $d_{uv}$ to be the shortest path distance between $u$ and $v$
with edge lengths $x^*_e$. Run the [rounding algorithm](#alg-st-cut) for
$\operatorname{Metric-LP}$ on $d$ and let $(S,T)$ be its output. We set
$X_e = 1$ if $e$ crosses $(S,T)$ and $X_e = 0$ otherwise.

First, we argue that $X$ is feasible. The constraints on $x^*$ ensure
that $d_{st} \geq 1$ and thus, $S$ always contains $s$ and never
contains $t$. So, removing the edges that cross $(S,T)$ disconnects $s$
and $t$, and thus $X$ is feasible.

Let $F$ be the edges that cross $(S,T)$. The expected cost of $X$ is
$$E\left[\sum_{e \in E} X_ec_e\right] = \sum_{e \in E} \Pr[e \in F]\cdot c_e.$$

We now upper bound $\Pr[e \in F]$. Suppose $e = (u,v)$ with
$d_{su} \leq d_{sv}$. Then, $u \in S$ and $v \in T$ if and only if
$d_{su} \leq \Theta < d_{sv}$. So,
$$\Pr[e \in F]
&= d_{sv} - d_{su} \\
&\leq d_{uv} \leq x^*_e.$$
The second-last inequality follows from the fact that shortest path
distances satisfy triangle inequality and the last inequality follows
from the fact that the edge $(u,v)$ is a path between $u$ and $v$.

Therefore, the expected cost of $X$ is
$$\sum_{e \in E} \Pr[e \in F]\cdot c_e \leq \sum_{e \in E} x^*_ec_e,$$
as desired.

:::

## Vertex Cover LP for Bipartite Graphs

In the remaining questions, we develop an exact rounding algorithm for
the vertex cover LP for bipartite graphs $G$. This shows that the
integrality gap of the vertex cover LP is $1$ in bipartite graphs. Note:
these questions appeared on the exam last year.

::: {exercise label=ex-8-3}

Consider the following simple randomized rounding algorithm: choose a
threshold $\tau$ uniformly at random from $[0,1]$, and for each vertex
$v$, choose $v$ to be in the solution $C$ if $y_v \geq \tau$. Show that
the expected size of $C$ is equal to $\sum_{v \in V} y_v$.

:::

::: {solution class=dropdown} ex-8-3

The probability that $v$ is chosen is exactly the probability that
$\tau$ is between $0$ and $y_v$ which in turn is equal to $y_v$. Thus,
by linearity of expectation, the expected size of $C$ is
$\sum_{v \in V} y_v$.

:::

::: {exercise label=ex-8-4}

Show that for every bipartite graph $G$, there is a feasible LP solution
for which the probability that the above rounding algorithm yields an
infeasible solution (i.e. $C$ does not cover every edge) is at least
$1/2$.

:::

::: {solution class=dropdown} ex-8-4

Consider the LP solution $y$ with $y_v = 1/2$ for every vertex $v$. The
above rounding algorithm returns the empty set when $\tau > 1/2$, which
happens with probability $1/2$.

:::

::: {exercise label=ex-8-5}

Show that there is a randomized rounding algorithm that given a feasible
LP solution $y$ produces a vertex cover whose expected cost is at most
$\sum_{v \in V} y_v$ and is feasible with probability $1$.

:::

::: {hint class=dropdown}

Choose a random threshold as above, but use a different condition for
when to include a vertex in $C$.

:::

::: {solution class=dropdown} ex-8-5

Choose $\tau$ uniformly at random from $[0,1]$. For every vertex $u$ on
the left side of $G$, choose $u$ to be in the solution $C$ if
$\tau \in [0,y_u]$. For every vertex $v$ on the right side of $G$,
choose $v$ to be in the solution $C$ if $\tau \in [1-y_v,1]$. Its
expected cost is at most $\sum_v y_v$ as for every vertex $v$, the
probability that it is in $C$ is $y_v$. It is feasible with probability
$1$, because for every edge $(u,v)$, the union of the intervals
$[0, y_u]$ and $[1-y_v,1]$ cover $[0,1]$ as $y_u+y_v\geq 1$.

:::

::: {exercise label=ex-8-6}

Show that there is an optimal solution $y$ to the LP that is integral,
i.e. $y_v$ is either $0$ or $1$ for every vertex $v \in V$. You may
assume the existence of the randomized rounding algorithm in @ex-8-5.

:::

::: {solution class=dropdown} ex-8-6

Let $y^*$ be an optimal LP solution. Applying the randomized rounding
algorithm of @ex-8-5 to $y^*$ gives a probability distribution over
vertex covers whose expected size is at most the cost of $y^*$. Thus, at
least one of them must have size at most the cost of $y^*$.

:::

[^1]: In the next lecture, we will see that the LP can still be solved
    efficiently despite having an exponential number of constraints.
