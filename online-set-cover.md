(sec-online-set-cover)=

# Online Set Cover

In the Online Set Cover problem, we are given a universe $U$ of $n$
elements and a collection of $m$ sets $S_1, \ldots, S_m \subseteq U$.
Unlike the usual Set Cover problem, we only need to cover a subset of
the $n$ elements, which is revealed online, one at a time. In
particular, at each timestep, an element $e_j$ arrives; if it is not
already covered by the sets we have chosen so far, then we have to buy
one of the sets covering it; buying decisions are irrevocable. The goal
is to minimize the number of sets bought.

To illustrate the problem, consider a telecommunications provider that
has to activate cell towers to provide service to clients that arrive
over time. When a client arrives, the provider has to activate a cell
tower near the client, and pay the cost of activating the cell tower.
Here, the cell towers correspond to sets and clients correspond to
elements that need to be covered.

:::: {figure label=fig-cell-coverage}

::: {image width=50%} ./w11-set-cover.png

:::

Illustration of online set cover as cell coverage. The sets
$S_1, S_2, S_3$ correspond to cell towers and the elements $e_1,e_2,e_3$
correspond to clients. The dashed lines represent which tower can serve
which client.

::::

It is also useful to view the problem using the bipartite graph
representation of set cover. The left-hand side of the bipartite graph
represent sets. Then, vertices representing elements that need to be
covered arrive on the right-hand side over time. When a right-hand side
vertex arrives, we also see its edges to the left-hand side; then we buy
a left-hand side vertex to ensure that every right-hand side vertex
neighbors at least one of the left-hand side vertices that we have
bought.

:::: {figure}

::: {image width=30%} ./w11-set-cover-bip.png

:::

Bipartite graph representation of @fig-cell-coverage.

::::

# Deterministic Lower Bound

To get a feel for the problem, we start with a lower bound against
deterministic algorithms.

::: {prf:theorem label=thm-osc-det-lb}

Every deterministic algorithm for Online Set Cover has competitive ratio
at least $m$, where $m$ is the number of sets.

:::

::: {prf:proof enumerated=false label=prf-osc-det-lb}

The input is constructed adversarially so that there is a single set
that covers every element but the algorithm receives very little
information which set it is, except that it is not one of the sets it
has chosen so far.

The first element $e_1$ is contained in all $m$ sets. Each subsequent
element $e_j$ is contained in every set except the ones the algorithm
has chosen so far. Once the algorithm has picked all $m$ sets, no
further elements arrive.

To show that the competitive ratio is $m$, it suffices to show that
there is a single set containing all elements. Suppose the last element
is $e_k$ and let $S_k$ be one of the sets containing it. By definition
of $e_k$, the set $S_k$ was not yet chosen by the algorithm when $e_k$
arrived, and so it was not yet chosen when $e_j$ arrived, for every
element $e_j$. Thus, for every $j \leq k$, by definition of $e_j$, $S_k$
also contains element $e_j$. Therefore $S_k$ contains all elements, as
desired.

:::

On the other hand, it is easy to see that **every** deterministic
algorithm has competitive ratio at most $m$: every algorithm buys at
most $m$ sets, and the optimal solution has to buy at least $1$ set.

::: {prf:observation label=obs-osc-det-ub}

Every deterministic algorithm for Online Set Cover has competitive ratio
at most $m$.

:::

Next, we turn to randomized algorithms.

# Randomized Algorithm

::: {prf:theorem label=thm-osc-rand}

There is a randomized polynomial-time algorithm for Online Set Cover
with expected competitive ratio $O(\log m \log n)$.

:::

The approach has two parts:

1.  We obtain a $O(\log m)$ competitive algorithm for a fractional
    version of the Online Set Cover problem
2.  Then we round the fractional solution losing a factor of $O(\log n)$

More precisely, Online Fractional Set Cover is the fractional relaxation
of Online Set Cover where we are allowed to buy fractions of sets and
for each element, the total purchased fraction of sets covering the
element is at least 1. More precisely, we have a variable
$x_i \in [0,1]$ for each set $S_i$. Initially, these variables are set
to 0. In each timestep, an element $e_j$ arrives and we buy additional
fractions of sets, i.e. raise the $x$ variables, so that
$\sum_{i : S_i \ni e_j} x_i \geq 1$. The decision to raise a variable is
irrevocable: we are not allowed to decrease variables. The goal is to
minimize $\sum_{i \in [m]} x_i$.

For this problem, we use the optimal offline fractional solution as a
benchmark, i.e. an $α$-competitive algorithm is one whose cost is at
most $\alpha$ of the optimal offline fractional solution.

At a high level, the randomized algorithm for Online Set Cover is as
follows: when element $e_j$ arrives, update the fractional solution
using an algorithm for Online Fractional Set Cover, and then based on
the updated values of the variables, decide which set to buy.

## Online Fractional Set Cover

We first show a lower bound.

::: {prf:theorem label=thm-osc-frac-lb}

Every algorithm for Online Fractional Set Cover has competitive ratio
$\Omega(\log m)$.

:::

::: {prf:proof enumerated=false}

Let us try to follow the [adversarial construction](#prf-osc-det-lb) for
the [deterministic lower bound](#thm-osc-det-lb). The first element
$e_1$ is contained in every set, and the algorithm raises the $x$
variables so that

$$\begin{equation}
\label{eq:osc-frac-avg}
\sum_{i \in [m]} x_i \geq 1.
\end{equation}$$

The difficulty now is that in the previous construction, $e_2$ is
contained in every set that has not yet been bought by the algorithm,
however, this is ill-defined since the algorithm is allowed to buy
fractions of sets. Instead, by an averaging argument @eq:osc-frac-avg
implies that there is one set $S_i$ such that $x_i \geq 1/m$. Then, the
next element $e_2$ that arrives is contained in every set except $S_i$.
After the algorithm updates the $x$ variables, since $e_2$ is contained
in $m-1$ sets, the averaging argument implies that one of those sets
must have been to an extent of $1/(m-1)$.

More precisely, the adversarial construction uses a marking procedure as
follows:

- Initially, every set is unmarked.
- While there is an unmarked set,
  - the next element $e_j$ that arrives is contained in every unmarked
    set
  - let $k$ be the number of unmarked sets
  - after the algorithm updates its $x$ variables, mark an arbitrary set
    $S_i$ with $x_i \geq 1/k$

We now show that the competitive ratio of the algorithm is at least
$\Omega(\log m)$ by showing that there is one set that contains all the
elements and that the cost of the algorithm is at least the $m$-th
harmonic number $H_m = \sum_{i=1}^m 1/i$. Suppose there are a total of
$k$ elements $e_1, \ldots, e_k$ that arrived. As in the
[proof](#prf-osc-det-lb) for the [deterministic lower
bound](#thm-osc-det-lb), every set that contains $e_k$ also contains
$e_1,\ldots, e_{k-1}$. Thus, optimal fractional solution has cost 1.

We now show that $\sum_{i \in [m]} x_i \geq \sum_{i \in [m]} 1/i$.
Re-index the sets so that $S_i$ is the $i$-th set to be marked by the
adversary. Thus, at the start of the timestep in which $S_i$ was marked,
at most $i-1$ sets were marked, and so $x_i \geq 1/(m-i+1)$. Therefore,
$$\sum_{i \in [m]} x_i \geq \sum_{i \in [m]} \frac{1}{m-i-1} = H_m = \Omega(\log m).$$

:::

To get some intuition for the fractional algorithm, observe that the
adversarial construction in the lower bound proof has a single unknown
set $S_{i^*}$ that contains all elements. Thus, a good fractional
algorithm must raise variables in such a way that that $x_{i^*}$ gets
increased to $1$ rapidly. This leads us to the following Multiplicative
Weights Update algorithm which doubles the fraction of every set
containing the uncovered element until it is covered.

::: {prf:algorithm label=alg-osc-frac}

- Initialize $x_i = 1/m$ for every $i \in [m]$
- **foreach** element $e_j$ **do**:
  - **while** $\sum_{i : S_i \ni e_j} x_i < 1$ **do**
    - $x_i \leftarrow 2 x_i$ for each $i$ such that $S_i \ni e_j$

:::

We now show that the algorithm is $O(\log m)$ competitive.

::: {prf:theorem label=thm-osc-frac-ub}

@alg-osc-frac is $O(\log m)$ competitive for Online Fractional Set Cover

:::

:::: {prf:proof enumerated=false}

We will prove that the algorithm is $O(\log m)$ competitive via
dual-fitting.

First, we construct the dual solution as follows. For each element
$e_j$, we set its dual variable to be the number of iterations of the
**while** loop in which the variables $x_i$ where $S_i \ni e_j$ are
doubled.

Next, we show that the dual pays for the primal:

$$\begin{equation}
\label{eq:dual-pays}
\sum_i x_i \leq \sum_j y_j.
\end{equation}$$

Let $T$ be the number of iterations of the **while** loop. Let $x_i(t)$
be the state of variable $x_i$ at the end of the iteration, and
$\delta_i(t) = x_i(t+1) - x_i(t)$ be the increase of the variable $x_i$
in iteration $t$. By definition, $\sum_j y_j = T$ and
$\sum_i x_i = \sum_{1 \leq t \leq T} \sum_i \delta_i(t)$. We now argue
that for every iteration $t$ of the **while** loop, the increase in the
primal variables is at most 1: $\sum_i \delta_i(t) \leq 1$.

Suppose that at the start of iteration $t$, we have
$\sum_{i : S_i \ni e_j} x_i(t-1) < 1$. Then, for each $i$ such that
$S_i \ni e_j$, we have $x_i(t) = 2x_i(t-1)$ and so
$\delta_i(t) = x_i(t-1)$. Thus, the total increase in the primal
variables in iteration $t$ is
$$\sum_i \delta_i(t) = \sum_{i : S_i \ni e_j} x_i(t-1) < 1,$$
as desired.

We now have

$$\begin{align*}
\sum_i x_i
= \sum_{1 \leq t \leq T} \sum_i \delta_i(t)
\leq T = \sum_j y_j,
\end{align*}$$

and so we have proved @eq:dual-pays.

Finally, we show that the dual $y$ violates the dual constraints by at
most a factor of $O(\log m)$. Consider the dual constraint for set
$S_i$:
$$\sum_{j : e_j \in S_i} y_j \leq 1.$$
The LHS is exactly the number of iterations of the **while** loop in
which the primal variable $x_i$ is doubled. We have that $x_i$ was
initialized to $1/m$. Observe also that once $x_i \geq 1$, then in the
future, all elements $e_j \in S_i$ are fully covered, and so $x_i$ will
no longer be doubled. Therefore, the total number of times $x_i$ is
doubled is at most $O(\log m)$ and so
$$\sum_{j : e_j \in S_i} y_j \leq O(\log m).$$

We have now shown that the dual pays the primal and the dual constraints
are violated by at most a factor of $O(\log m)$. Thus, we conclude that
the algorithm is $O(\log m)$ competitive.

::::

Next, we show that we can round $x$ in an online fashion.

## Online Randomized Rounding

The rounding algorithm is essentially an online adaptation of the
[rounding algorithm for offline set cover](#alg-set-cover-2): for each
set $S_i$, we pick a threshold $\Theta$ uniformly at random from the
interval $[0,1]$; then once the variable $x_i$ increases to at least
$\Theta/\log n$, we buy the set $S_i$.

Putting it all together, the randomized algorithm for Online Set Cover
is as follows.

::: {prf:algorithm label=alg-osc}

- Initialize $x_i = 1/m$ for every $i \in [m]$
- Choose $\Theta_i$ uniformly at random from $[0,1]$ for every
  $i \in [m]$ independently
- **foreach** element $e_j$ **do**:
  - *(Update fractional solution)*
  - **while** $\sum_{i : S_i \ni e_j} x_i < 1$ **do**
    - $x_i \leftarrow 2 x_i$ for each $i$ such that $S_i \ni e_j$
  - *(Randomized rounding)*
  - **foreach** $i \in [m]$ **do**
    - Buy $S_i$ if not yet bought and $(\ln n) x_i \geq \Theta$
  - **endfor**
  - If $e_j$ not covered, buy any set containing $e_j$

:::

Observe that for each set $S_i$, the probability that @alg-osc buys the
set is exactly the same as the [offline rounding
algorithm](#alg-set-cover-2) on the fractional solution $x$ with
$\alpha = \ln n$. Thus, we can use the same analysis to argue that its
expected cost is at most $(\ln n) \sum_i x_i$. Since the latter is at
most $O(\log m)$ of the optimal fractional solution, we get that the
expected competitive ratio of @alg-osc is at most $O(\log m \log n)$,
giving us @thm-osc-rand.
