(sec-pd-shortest-paths)=

# Primal-Dual for Shortest Paths

In this section, we see how Dijkstra's algorithm for shortest paths can
be derived as a primal-dual algorithm.

::: {prf:definition label=prob-shortest-path}

The input is a directed graph $G = (V,E)$ with edge costs $c_e$, and two
vertices $s$ and $t$. The goal is to find the shortest path from $s$ to
$t$.

:::

## Primal and Dual LPs

It is not completely clear, at first sight, how to formulate an LP.
Naturally, we have a variable $x_e$ for each edge $e$. However, it is
unclear to ensure that they encode a path from $s$ to $t$.

Define a subset $S \subseteq V$ to be an $(s,t)$-cut if $s \in S$ and
$t \notin S$. Let $\mathcal{C}$ be the set of $(s,t)$-cuts. Moreover,
let $\delta(S) = \{(u,v) : u \in S, v \notin S\}$ be the set of edges
going out from $S$.

The idea is that a path $P$ from $s$ to $t$ must cross each $(s,t)$-cut
$S$ at least once, i.e. it must contain at least one edge going out from
$S$. This gives us the following LP and its dual.

$$\begin{align*}
\text{minimize} \quad & \sum_{e \in E} c_e x_e\\
\text{subject to} \quad & \sum_{e \in \delta(S)} x_e \geq 1 && \forall S \in \mathcal{C}\\
& x \geq 0
\end{align*}$$

$$\begin{align*}
\label{w10-moat-packing-dual}
\text{maximize} \quad & \sum_{S \in \mathcal{C}} y_S\\
\text{subject to} \quad & \sum_{S : e \in \delta(S)} y_S \leq c_e && \forall e \in E\\
& y \geq 0
\end{align*}$$

(sec-moat-interpretation)=

### Interpretation of Dual

Whenever we derive the dual of an LP, it is useful to try and think of
an intuitive interpretation and why the dual gives a lower bound on the
optimal integral solution. In this case, we think of the dual LP
@w10-moat-packing-dual as a "moat packing", where for each
$S \in \mathcal{C}$, we are placing a moat around $S$ of width $y_S$.

To get some intuition as to why a feasible dual gives a lower bound on
the shortest $(s,t)$-path, suppose that we have a feasible dual in which
exactly one variable is positive, i.e. $y_R > 0$ for some
$R \in \mathcal{C}$ and $y_S = 0$ for $S \in \mathcal{C}$ where
$S \neq R$. The value of the dual is exactly $y_R$. Then, the dual
feasibility constraints imply that $y_R \leq c_e$ for every
$e \in \delta(R)$, i.e. every edge $e \in \delta(R)$ has cost
$c_e \geq y_R$. Since every $(s,t)$-path has to cross the $(s,t)$-cut
$R$ at least once, it has to contain at least one edge of $\delta(R)$
and thus costs at least $y_R$. Therefore, $y_R$ is indeed a lower bound
on the cost of the shortest $(s,t)$-path. See @fig-moat-width for an
illustration.

:::: {figure label=fig-moat-width}

::: {image width=75%} ./moat-width.png

:::

In this example, $R = \{s,2,6,7\}$, $y_R = 17$, and
$\delta(R) = \{(2,3), (6,3), (6,5),(7,5),(7,t)\}$.

::::

## Which Dual Variables to Raise?

Following the approach in @sec-pd-framework, we start with an empty set
of edges and $y = 0$. Every primal constraint is violated. Unlike the
$f$-approximation for Set Cover in @sec-pd-setcover, we need to be
careful which dual variables we raise.

We draw inspiration from Dijkstra's algorithm. At a high level, the
algorithm maintains a set $S$ of "explored vertices" and a shortest-path
tree $T$ from $s$ to $S$. Initially, $S = \{s\}$ and $T = \emptyset$.
Then, the algorithm iteratively "grows" the shortest-path tree $T$ and
$S$ as follows:

- for each $u \in S$, let $d(u)$ be the distance from $s$ to $u$ in $T$
- let $u \in S$ and $v \notin S$ be vertices that minimize
  $d(u) + c_{uv}$
- add $v$ to $S$ and the edge $(u,v)$ to $T$

The algorithm stops once $t \in S$ and outputs the $(s,t)$-path in $T$.

:::: {figure}

::: {image width=75%} ./shortest-path-moat.png

:::

Illustration of an iteration of Dijkstra's algorithm.

::::

We will raise dual variables to mimic the shortest-path tree growing
process of Dijkstra's algorithm.

## Moat-Growing Algorithm

With respect to a dual solution $y$, we say that edge $e$ is *tight* if
the corresponding dual constraint is tight:
$\sum_{S : e \in \delta(S)} y_S = c_e$.

The primal-dual algorithm works as follows: it starts with the moat $S$
containing only $s$; in each iteration, it grows the dual variable $y_S$
until an edge $(u,v)$ becomes tight and then it adds $(u,v)$ to $T$ and
$v$ to $S$.

::: {prf:algorithm label=alg-pd-shortest-path}

- Initialize $T = \emptyset$, $y = 0$ and $S = \{s\}$
- **while** $t \notin S$ **do**
  - Raise $y_S$ until some edge $(u,v)$ tight
  - Add $(u,v)$ to $T$ and $v$ to $S$
- **return** $(s,t)$-path $P$ in $T$

:::

::: {aside}

See pages 31 - 48 of [Annotated Slides](./slides-post-w10.pdf) for a
step-by-step illustration of the algorithm.

:::

Let us now show that $P$ is the shortest path via complementary
slackness conditions. The primal complementary slackness conditions are:

$$\begin{equation}
\label{eq:shortest-path:primal-cs}
e \in P \implies \sum_{S : e \in S_i} y_S = c_e
\end{equation}$$

which says that we only take edge $e$ if the dual can pay for it. The
dual complementary slackness conditions are:

$$\begin{equation}
\label{eq:shortest-path:dual-cs}
y_S > 0 \implies |P \cap \delta(S)| = 1
\end{equation}$$

which says that for every moat $S$ that the algorithm grew, the path $P$
leaves $S$ at most once.

::: {prf:lemma label=lem-shortest-path-cs}

The primal-dual pair $F$ and $y$ constructed by @alg-pd-shortest-path
satisfies primal and dual complementary slackness conditions.

:::

::::: {prf:proof enumerated=false}

Since $P$ is a subset of $T$ and we only add $e$ to $T$ if
$\sum_{S : e \in S_i} y_S = c_e$, primal complementary slackness
conditions are satisfied.

Suppose, towards a contradiction, that dual complementary slackness is
not satisfied. Let $S$ be such that $y_S > 0$ and
$|P \cap \delta(S)| > 1$. We have that the path $P$ left $S$, reentered
$S$ and then left again, possibly reentering and leaving multiple times.
See @fig-shortest-path-cs.

:::: {figure label=fig-shortest-path-cs}

::: {image width=75%} ./shortest-path-moat-cs.png

:::

Illustration of the argument that if $|P \cap \delta(S)| > 1$, then
there must be at least one edge in $T$ that enters $S$. In this example,
the shaded region is $S$, the bold edges are edges of $T$, and the edge
$(4,3)$ is the edge of $T$ entering $S$.

::::

Consider the iteration in which the algorithm grew $y_S$. At the start
of the iteration, there were no edges in $T$ leaving $S$, and at the end
of the iteration, an outgoing edge was added to $T$. Moreover, in each
future iteration, the algorithm grows a moat $S'$ containing $S$ and
adds an edge leaving $S'$. Thus, it is not possible for an edge to be
added that goes into $S$, a contradiction.

:::::
