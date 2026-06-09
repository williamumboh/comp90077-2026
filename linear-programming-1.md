(sec-linear-programming-1)=

# Linear Programming, or: How I Learnt to Stop Worrying and Relax (Fractionally)

Many of the problems seen so far can be cast in terms of finding an
assignment to Boolean variables that maximize or minimize a linear
function of the variables subject to linear inequalities on the
variables.

::: {prf:example} Vertex Cover

Let $G = (V,E)$ be a graph. We have a Boolean variable $x_v$ for each
vertex $v$. Assigning a value of $1$ to $x_v$ means we choose $v$ and
assigning a value of $0$ means we do not. The number of vertices we
choose is then $\sum_v x_v$. For each edge $(u,v) \in E$, the constraint
that either $u$ or $v$ is chosen is equivalent to the inequality
$x_u + x_v \geq 1$.

In other words, we can express vertex cover as follows:
$$\text{minimize} \quad & \sum_v x_v\\
\text{subject to} \quad & x_u + x_v \geq 1 \quad  && \forall (u,v) \in E\\
& x_v \in \{0,1\} \quad  && \forall v \in V$$

:::

::: {prf:example} Set Cover

Let $U$ be a universe of $n$ elements and $S_1, \ldots, S_m$ be subsets
of $U$. We have a Boolean variable $x_i$ for each $i \in [m]$. Assigning
a value of $1$ to $x_i$ means we choose $S_i$ and assigning a value of
$0$ means we do not. The number of sets we choose is then $\sum_i x_i$.
For each element $e \in U$, the constraint that some set containing $e$
is chosen is equivalent to the inequality
$\sum_{i : S_i \ni e} x_i \geq 1$.

Thus, we can express set cover as follows:
$$\text{minimize} \quad & \sum_i x_i\\
\text{subject to} \quad & \sum_{i: S_i \ni e} x_i \geq 1 \quad  && \forall e \in U\\
& x_i \in \{0,1\} \quad  && \forall i \in [m]$$

:::

::: {prf:example} Matching

Let $G = (V,E)$ be graph. We have a Boolean variable $x_e$ for each edge
$e \in E$. Assigning a value of $1$ to $x_e$ means we choose $e$ and
assigning a value of $0$ means we do not. The number of edges we choose
is $\sum_e x_e$. For each vertex $v \in V$, the constraint that at most
one edge in $M$ is incident to $v$ is equivalent to the inequality
$\sum_{e \text{ incident to } v} x_e \leq 1$.

Thus, we can express maximum matching as follows. Let
$\delta(v) = \{(u,v) \in E\}$, the set of edges incident to $v$.
$$\text{maximize} \quad & \sum_e x_e\\
\text{subject to} \quad & \sum_{e \in \delta(v)} x_e \leq 1 \quad  && \forall v \in V\\
& x_e \in \{0,1\} \quad  && \forall e \in E$$

:::

::: {prf:example} Knapsack

We are given $n$ items, each with weight $w_i$ and size $s_i$, and a
capacity $C$. We have a Boolean variable $x_i$ for each item
$i \in [n]$. Assigning a value of $1$ to $x_i$ means we choose item $i$
and assigning a value of $0$ means we do not. The total weight of chosen
items is $\sum_i w_i x_i$. The capacity constraint is equivalent to the
linear inequality $\sum_i s_i x_i \leq C$.

Thus, we can express knapsack as follows:
$$\text{maximize} \quad & \sum_i w_ix_i\\
\text{subject to} \quad & \sum_i s_ix_i \leq C \quad  && \\
& x_i \in \{0,1\} \quad  && \forall i \in [n]$$

:::

## Linear Programs

The above are examples of *linear programs*.

::: {prf:definition label=def-lp} Linear Program

A *linear program* consists of:

- a set of variables $x_1, \ldots, x_n$,
- coefficients $a_1, \ldots, a_n$ for the objective,
- a set of linear inequalities
  $$   b_{11}x_1 + \ldots + b_{1n}x_n &\leq c_1\\
     &\vdots\\
     b_{m1}x_1 + \ldots + b_{mn}x_n &\leq c_m\\
     $$

The goal is to find an assignment of values to variables
$x_1,\ldots,x_n$ satisfying the linear inequalities so as to
maximize/minimize $\sum_i a_ix_i$.

:::

We can succinctly represent $\sum_i a_ix_i$ as $a^\intercal x$ where $a$
is the vector $(a_1, \ldots, a_n)$ and $x$ is the vector
$(x_1, \ldots, x_n)$. Let $B$ be the matrix where the $(i,j)$ entry is
$b_{ij}$ and $c$ be the vector $(c_1, \ldots, c_m)$. Then, the
inequalities can represented as $Bx\leq c$. Thus, we get the following
equivalent definition.

::: {prf:definition label=def-lp-vec} Linear Program

Given vectors $a \in \mathbb{R}^n$ and $c \in \mathbb{R}^m$, and a
matrix $B \in \mathbb{R}^{m \times n}$, the goal is to solve the
following optimization problem
$$\text{maximize} \quad & a^\intercal x\\
\text{subject to} \quad & Bx \leq c \quad  & \\
& x \in \mathbb{R}^n \quad  &$$

:::

When the entries of $x$ are restricted to integers, e.g. $\{0,1\}$, then
it is called an *integer linear program* or *integer program*, for
short.

Since integer programs capture NP-hard problems such as vertex cover,
solving integer programs is also NP-hard. Fortunately, linear programs
can be solved efficiently.

::: {prf:theorem label=thm-lp-solve}

Given a linear program with $n$ variables, $m$ constraints, and whose
coefficients can be represented using $L$ bits, an optimal solution can
be found in time polynomial in $n, m, L$.

:::

Next, we show how to use this fact to design approximation algorithms by
"rounding" solutions to linear programs.

(sec-vc-lp)=

## Rounding Vertex Cover LP

Recall the integer program for vertex cover from earlier
$$\text{minimize} \quad & \sum_v x_v\\
\text{subject to} \quad & x_u + x_v \geq 1 \quad  && \forall (u,v) \in E\\
& x_v \in \{0,1\} \quad  && \forall v \in V$$
The following is a linear program for vertex cover:
$$\text{minimize} \quad & \sum_v x_v\\
\text{subject to} \quad & x_u + x_v \geq 1 \quad  && \forall (u,v) \in E\\
& x_v \geq 0 \quad  && \forall v \in V$$
Observe that the linear program *relaxes* the integer constraint
$x_v \in \{0,1\}$ to $x_v \geq 0$ which allows $x_v$ to take fractional
values such as $1/4$.

::: {prf:lemma label=lem-frac-vc}

Let $\operatorname{OPT}_{LP}$ and $\operatorname{OPT}_{IP}$ be the
values of the optimal solutions to the linear and integer programs,
respectively. Let $C^*$ be a minimum vertex cover. Then, we have
$$\operatorname{OPT}_{LP} \leq \operatorname{OPT}_{IP} = |C^*|.$$

:::

::: {prf:proof enumerated=false}

The first inequality follows from the fact that every feasible solution
to the integer program is also feasible for the linear program. The
second follows from the fact that there is a one-to-one correspondence
between feasible integer solutions and vertex covers.

:::

Now, compute an optimal solution $x^*$ to the linear program. For every
edge $(u,v) \in E$, we have $x^*_u + x^*_v \geq 1$, and so either
$x^*_u \geq 1/2$ or $x^*_v \geq 1/2$. Thus, if we take $C$ to be the set
of vertices $v$ such that $x^*_v \geq 1/2$, then $C$ is a vertex cover.
Intuitively, we are rounding the variables to the nearest integer.

What about the size of $C$? We get that
$$| C |
= ∑_{v ∈ C} 1
&≤ 2∑_{v ∈ C} x^*_v \\
&≤ 2∑_v x^*_v \\
&= 2\operatorname{OPT}_{LP}\\
&≤ 2|C^*|.$$

One may object that this is a more complicated way of getting the same
approximation ratio as the previous algorithm in @sec-matching-vc.
However, what is impressive is that we get for free the weighted version
of vertex cover.

::: {prf:definition label=prob-vc-wt} Weighted Vertex Cover

Given a graph $G = (V,E)$ and vertex weights $w_v$, find a vertex cover
$C$ that minimizes $\sum_{v \in C} w_v$.

:::

The following are the integer and linear programs.
$$\text{minimize} \quad & \sum_v w_v x_v\\
\text{subject to} \quad & x_u + x_v \geq 1 \quad  && \forall (u,v) \in E\\
& x_v \in \{0,1\} \quad  && \forall v \in V$$

$$\text{minimize} \quad & \sum_v w_v x_v\\
\text{subject to} \quad & x_u + x_v \geq 1 \quad  && \forall (u,v) \in E\\
& x_v \geq 0 \quad  && \forall v \in V$$

The proof of the following lemma follows along the same lines as above.

::: {prf:lemma label=lem-frac-vc-wt}

Let $\operatorname{OPT}_{LP}$ and $\operatorname{OPT}_{IP}$ be the
values of the optimal solutions to the linear and integer programs,
respectively. Let $C^*$ be a minimum-weight vertex cover. Then, we have
$$\operatorname{OPT}_{LP} \leq \operatorname{OPT}_{IP} = \sum_{v \in C^*} w_v.$$

:::

Now suppose we have an optimal solution $x^*$ to the linear program.
Take $C = \{v : x_v \geq 1/2\}$. The same reasoning as before implies
that $C$ is a vertex cover. What about its weight? Since
$x^*_v \geq 1/2$ for each $v \in C$, we have
$$\sum_{v \in C} w_v
&\leq 2\sum_{v \in C} w_v x^*_v \\
&= 2\operatorname{OPT}_{LP}\\
&\leq 2\sum_{v \in C^*} w_v.$$

Et voila, we get a $2$-approximation for weighted vertex cover by only
doing slightly more work than for vertex cover!

::: {prf:theorem label=thm-weighted-vc}

There is a $2$ approximation algorithm for Weighted Vertex Cover.

:::
