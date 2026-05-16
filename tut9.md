(sec-tut9)=

# Tutorial 9

::: {exercise label=ex-9-1}

Consider the linear program
$$    \label{ex-9-1-lp}
        \text{maximize} \quad & x-2z\\
        \text{subject to} \quad
        &x-y  \leq 1\\
        &2y-z  \leq 1\\
        &x,y,z \geq 0\,.
        $$

Construct the dual linear program.

:::

::: {solution class=dropdown} ex-9-1

The dual is
$$        \text{minimize} \quad & u+v\\
        \text{subject to} \quad
        & u \geq 1\\
        & v \leq 2\\
        & -u + 2v  \geq 0 \\
        & u,v \geq 0\,.
        $$ The solution $(u,v)=(1,1/2)$ is feasible for the dual. Its
objective function value is $3/2$, which equals the primal objective
function value for $(x,y,z) = (3/2,1/2,0)$. Hence these are optimal
solutions by [weak LP duality](#lp-weak-duality).

:::

::: {exercise label=ex-9-2}

Show that the solution $(x,y,z) = (3/2,1/2,0)$ to LP @ex-9-1-lp is
optimal.

:::

::: {hint class=dropdown}

Construct a feasible dual solution and use [weak LP
duality](#lp-weak-duality).

:::

::: {solution class=dropdown} ex-9-2

The dual is
$$        \text{minimize} \quad & u+v\\
        \text{subject to} \quad
        & u \geq 1\\
        & v \leq 2\\
        & -u + 2v  \geq 0 \\
        & u,v \geq 0\,.
        $$ The solution $(u,v)=(1,1/2)$ is feasible for the dual. Its
objective function value is $3/2$, which equals the primal objective
function value for $(x,y,z) = (3/2,1/2,0)$. Hence these are optimal
solutions by [weak LP duality](#lp-weak-duality).

:::

::: {exercise label=ex-9-3}

Recall the linear program for [Interval Hitting
Set](#prob-interval-hitting). We have a variable $x_i$ for each
$i \in [n]$ to indicate whether or not we choose $i$ to be in the
hitting set. The LP is as follows.

$$\begin{align}
\text{minimize} \quad & \sum_i x_i\\
\text{subject to} \quad & \sum_{i \in I_j} x_i \geq 1 \quad  && \forall j \in [m]\\
& x_i \geq 0 \quad  && \forall i \in [n]
\end{align}$$

Derive the dual linear program.

:::

::: {solution class=dropdown} ex-9-3

Following the usual recipe for deriving duals, we first introduce a dual
variable $y_j$ for each primal constraint $\sum_{i \in I_j} x_i \geq 1$.
The coefficient of $y_j$ in the dual objective is $1$ since the RHS of
the corresponding primal constraint is $1$. Thus, the dual objective is
$\sum_{j \in [m]} y_j$.

Next, for each primal variable $x_i$, we introduce a dual constraint
$\sum_{j : I_j \ni i} y_j \leq 1$. The RHS is the coefficient of $x_i$
in the primal objective, which is $1$. The LHS is because the primal
variable $x_i$ appears in primal constraints $j$ such that $I_j \ni i$
with coefficient $1$.

Thus, the dual LP is:

$$\begin{align}
\text{maximize} \quad & \sum_j y_j\\
\text{subject to} \quad & \sum_{j : I_j \ni i} y_j \leq 1 \quad  && \forall i \in [n]\\
& y_j \geq 0 \quad  && \forall j \in [m]
\end{align}$$

Observe that this is exactly the LP for [Disjoint
Intervals](#prob-disjoint-intervals) we derived in [Tutorial 6 Exercise
3](#ex-6-3), and so these problems are dual to each other.

:::

::: {exercise label=ex-9-6}

Derive the dual for the weighted vertex cover LP

$$\begin{align}
\text{minimize} \quad & \sum_v w_vy_v\\
\text{subject to} \quad & y_u + y_v \geq 1 \quad  && \forall (u,v) \in E\\
& y\geq 0
\end{align}$$

:::

::: {solution class=dropdown} ex-9-6

For each primal constraint $y_u + y_v \geq 1$, we introduce a dual
variable $x_e$ where $e = (u,v)$. The coefficient of $x_e$ in the dual
objective is $1$ since the RHS of the corresponding primal constraint is
$1$. Thus, the dual objective is $\sum_{e \in E} x_e$.

Next, for each primal variable $y_v$, we introduce a dual constraint
$\sum_{e \in \delta(v)} y_e \leq w_v$. The RHS is the coefficient of
$x_i$ in the primal objective, which is $w_v$. The LHS is because for
each primal variable $y_v$, it appears in the primal constraints
$e \in \delta(v)$ with coefficient $1$.

$$\begin{align}
\text{maximize} \quad & \sum_{e \in E} x_e\\
\text{subject to} \quad & \sum_{e \in \delta(v)} y_e \leq w_v \quad  && \forall (u,v) \in E\\
& x\geq 0
\end{align}$$

This corresponds to a "capacitated" variant of the matching problem
where each vertex $v$ has a capacity $w_v$ and can be incident to at
most $w_v$ edges.

:::

::: {exercise label=ex-9-4}

Derive the dual for the $\operatorname{Path-LP}$ relaxation for [Min
$(s,t)$-Cut](#prob-st-cut).
$$\text{minimize} \quad & \sum_{e \in E} c_ex_e\\
\text{subject to} \quad & \sum_{e \in P} x_e \geq 1 \quad  && \forall P \in \mathcal{P}_{s,t}\\
& x \geq 0 \quad$$

Recall that $\mathcal{P}_{s,t}$ is the set of $(s,t)$-paths.

:::

::: {solution class=dropdown} ex-9-4

For each primal constraint $\sum_{e \in P} x_e \geq 1$, we introduce a
dual variable $y_P$. In other words, we have a dual variable for each
$P \in \mathcal{P}_{s,t}$. The coefficient of $y_P$ in the dual
objective is $1$ since the RHS of the corresponding primal constraint is
$1$. Thus, the dual objective is $\sum_{P \in \mathcal{P}_{s,t}} y_P$.

Next, for each primal variable $x_e$, we introduce a dual constraint
$\sum_{P \in \mathcal{P}_{s,t} : P \ni e} y_P \leq c_e$. The RHS is the
coefficient of $x_e$ in the primal objective, which is $1$. The LHS is
because each primal variable $x_e$ appears with coefficient $1$ in the
primal constraints $P \in \mathcal{P}_{s,t}$ such that $e \in P$.

$$\begin{align}
\text{maximize} \quad & \sum_{P \in \mathcal{P}_{s,t}} y_P\\
\text{subject to} \quad &  \sum_{P \in \mathcal{P}_{s,t} : P \ni e} y_P \leq c_e \quad  && \forall e \in E\\
& y\geq 0
\end{align}$$

This corresponds to the Max-Flow problem where we want to sent as much
flow from $s$ to $t$ along paths subject to capacity constraints on the
edges.

:::
