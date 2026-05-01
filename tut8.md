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

The integer program is as follows.
$$\text{minimize} \quad & \sum_{e \in E} x_ec_e\\
\text{subject to} \quad & \sum_{e \in P} x_e \geq 1 \quad  && \forall P \in \mathcal{P}_{s,t}\\
& x_e \in \{0,1\} \quad  && \forall e \in E$$

Show that the LP relaxation of the above IP is equivalent to the one in
@sec-cut-IP. In other words:

- for every feasible solution $x$ to the LP relaxation of the above IP,
  there is a feasible solution $d$ to the LP relaxation in @sec-cut-IP
  with cost $\sum_{(u,v) \in E} d_{uv} c_uv = \sum_{e \in E} x_ec_e$ and
- for every feasible solution $d$ to the LP relaxation in @sec-cut-IP,
  there is a feasible solution $x$ to the LP relaxation of the above IP
  with cost $\sum_{e \in E} x_ec_e = \sum_{(u,v) \in E} d_{uv} c_uv$.

:::

::: {exercise label=ex-8-2}

Assume you are given an optimal solution $x^*$ to the LP[^1]. Show that
you can round it to an integral solution $X$ whose cost is at most that
of $x^*$.

:::

::: {hint class=dropdown}

Interpret the variables $x_e$ as edge lengths.

:::

::: {hint class=dropdown}

Pick a ball with random radius centered at $s$.

:::

## Vertex Cover LP for Bipartite Graphs

In the remaining questions, we consider the vertex cover LP for
bipartite graphs $G$. Note: these questions appeared on the exam last
year.

::: {exercise}

Consider the following simple randomized rounding algorithm: choose a
threshold $\tau$ uniformly at random from $[0,1]$, and for each vertex
$v$, choose $v$ to be in the solution $C$ if $y_v \geq \tau$. Show that
the expected size of $C$ is equal to $\sum_{v \in V} y_v$.

:::

::: {exercise}

Show that for every bipartite graph $G$, there is a feasible LP solution
for which the probability that the above rounding algorithm yields an
infeasible solution (i.e. $C$ does not cover every edge) is at least
$1/2$.

:::

::: {exercise}

Show that there is a randomized rounding algorithm that given a feasible
LP solution $y$ produces a vertex cover whose expected cost is at most
$\sum_{v \in V} y_v$ and is feasible with probability $1$.

:::

::: {hint class=dropdown}

Choose a random threshold as above, but use a different condition for
when to include a vertex in $C$.

:::

::: {exercise}

Show that there is an optimal solution $y$ to the LP that is integral,
i.e. $y_v$ is either $0$ or $1$ for every vertex $v \in V$. You may
assume the existence of the randomized rounding algorithm in part d.

:::

[^1]: In the next lecture, we will see that the LP can still be solved
    efficiently despite having an exponential number of constraints.
