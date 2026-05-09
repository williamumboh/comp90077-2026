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

::: {exercise label=ex-9-6}

Derive the dual for the weighted vertex cover LP
$$\text{minimize} \quad & \sum_v w_vy_v\\
\text{subject to} \quad & y_u + y_v \geq 1 \quad  && \forall (u,v) \in E\\
& y\geq 0$$

:::

::: {exercise label=ex-9-4}

Derive the dual for the $\operatorname{Path-LP}$ relaxation for [Min
$(s,t)$-Cut](#prob-st-cut).
$$\text{minimize} \quad & \sum_{e \in E} c_ex_e\\
\text{subject to} \quad & \sum_{e \in P} x_e \geq 1 \quad  && \forall P \in \mathcal{P}_{s,t}\\
& x \geq 0 \quad$$

:::

::: {exercise label=ex-9-5}

Derive the dual for the following variant of the
$\operatorname{Metric-LP}$ relaxation for [Min
$(s,t)$-Cut](#prob-st-cut).
$$\text{minimize} \quad & \sum_{(u,v) \in E} c_{uv}d_{uv}\\
\text{subject to} \quad
& d_{st} \geq 1 \quad  && \\
& d_{uv} \leq d_{uw} + d_{vw} && \forall u,v,w \in V\\
& d \geq 0$$

:::

::: {hint class=dropdown}

First transform the inequalities so that they are all $\geq$ and the
variables are all on the LHS.

:::
