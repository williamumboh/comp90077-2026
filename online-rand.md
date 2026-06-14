(sec-online-rand)=

# Randomized Online Algorithms

In the final section of this subject, we discuss randomized lower bounds
for Online Set Cover and a randomized algorithm for the tram waiting
problem.

## Randomized Lower Bounds for Set Cover

[Previously](#sec-online-set-cover), we [showed](#thm-osc-rand) that
there is a randomized polynomial-time algorithm for Online Set Cover
with expected competitive ratio $O(\log m\log n)$.

Can we do better? Unfortunately, it is NP-hard to better if we insist on
polynomial-time algorithms.

::: {prf:theorem label=thm-online-sc-hardness}

Every polynomial-time online algorithm for Online Set Cover has
competitive ratio $\Omega(\log m \log n)$ unless P = NP.

:::

What if we allow exponential-time algorithms? While exponential-time
algorithms are impractical, the point of the question is to understand
if the source of the difficulty is from the restriction to
polynomial-time algorithms or if it is purely due to the fact that
online algorithms have to make decisions without knowing the full input.

To the best of our knowledge, we only know of the following lower bound
on randomized algorithms.

::: {prf:theorem label=thm-online-sc-rand-lb}

Every randomized algorithm for Online Set Cover has competitive ratio
$\Omega(\log m)$.

:::

The proof for the lower bound follows from a more general result that
shows that fractional algorithms are at least as powerful as randomized
algorithms. Together with the [lower bound on fractional
algorithms](#thm-osc-frac-lb), we get the desired lower bound on
randomized algorithms.

::: {prf:theorem label=thm-online-sc-rand-frac}

If there is a randomized algorithm for Online Set Cover with expected
competitive ratio $\alpha$, then there is an algorithm for Online
Fractional Set Cover with competitive ratio $\alpha$.

:::

::: {prf:proof enumerated=false}

Let ALG be a randomized algorithm. We will show how to maintain an
online fractional solution $x$ whose cost is at most the expected cost
of ALG.

Let $X_i(j)$ be the random variable indicating whether ALG has bought
set $S_i$ after element $e_j$ has arrived, and let $X_i$ be the random
variable indicating whether ALG bought set $S_i$ after the last element
has arrived. Observe that the expected cost of ALG is exactly
$$E\left[\sum_{i\in [m]} X_i \right] = \sum_{i \in [m]} E[X_i],$$
by linearity of expectation.

Thus, a natural idea is to consider the online fractional solution $x$
in which $x_i$ is set to $E[X_i(j)]$, after element $e_j$ arrived.
Observe that we never decrease variables since, regardless of the
randomness, we always have $X_i(j) \geq X_i(j-1)$ and so
$E[X_i(j)] \geq E[X_i(j-1)]$. Note that the final value of $x_i$ is
exactly $E[X_i]$. Thus, the cost of the solution is
$$\sum_{i \in [m]} x_i = \sum_{i \in [m]} E[X_i],$$
which is exactly the expected cost of ALG.

:::

## Randomized Algorithm for Tram Waiting Problem

We now give another example of the framework that yielded the randomized
algorithm for Online Set Cover to get a randomized algorithm for the
Tram Waiting Problem.

::: {prf:theorem label=thm-online-ski-rand}

There is a randomized algorithm for the [Tram Waiting
Problem](#sec-tram-waiting) with expected competitive ratio
$\frac{e}{e-1}$.

:::

The approach has two parts:

1.  We obtain a $\frac{e}{e-1}$ competitive algorithm for a fractional
    version of the Tram Waiting problem
2.  Then we round the fractional solution exactly.

The fractional version of the Tram Waiting problem is related to its
linear program. In [Tutorial 11 Exercise 3](#ex-11-3), we derived the
following linear program for the offline Tram Waiting problem where the
tram arrival time $A$ is known. (See the exercise solution for a
detailed explanation of the linear program and its dual).

$$\begin{align}
\text{minimize} \quad & Wx_W + \sum_{1 \leq t \leq A} x_t\\
\text{subject to} \quad & x_t + x_W \geq 1 \quad  && \forall 1 \leq t \leq A-1\\
& x \geq 0 \quad  &&
\end{align}$$

The dual LP is as follows.

$$\begin{align}
\text{maximize} \quad & \sum_{1 \leq t \leq A-1} y_t\\
\text{subject to} \quad & y_t \leq 1 \quad  && \forall 1 \leq t \leq A-1\\
& \sum_{1 \leq t \leq A-1} y_t \leq W \quad  && \\
& y \geq 0 \quad  &&
\end{align}$$

The Online Fractional Tram Waiting problem is as follows. Initially,
every primal variable is set to 0. Then, for each timestep $t$ before
the (unknown) tram arrival time $A$, if $x_W$ is not 1, then we increase
$x_W$ and $x_t$ so that $x_W + x_t \geq 1$.

Here is the intuition for the fractional algorithm. Recall that for
deterministic algorithms, the key decision is when to walk. Thus for the
fractional algorithm, the key is in deciding how to increase $x_W$ over
time, and for each timestep, setting $x_t = 1 - x_W$ to satisfy the
constraint at timestep $t$.

As in Online Set Cover, we will be using a multiplicative update on
$x_W$. However, we will initialize $x_W$ to be 0. Thus, we will use a
slightly different update rule with an additional term $\alpha$. We will
analyze the competitive ratio in terms of $\alpha$ and then show that
for an appropriate setting of $\alpha$, we get the desired competitive
ratio.

::: {prf:algorithm label=alg-tram-frac}

- Initialize $x_W = 0$
- **foreach** timestep $t < A$ such that $x_W < 1$ **do**:
  - $x_W \leftarrow \left(1 + \frac{1}{W}\right) x_W + \alpha$
  - $x_t = 1 - x_W$

:::

We now show that the algorithm is $\frac{e}{e-1}$ competitive for an
appropriate choice of $\alpha$.

::: {prf:theorem label=thm-tram-frac-ub}

For some $\alpha$, @alg-tram-frac is $\frac{e}{e-1}$ competitive for
Online Fractional Tram Waiting.

:::

::: {prf:proof enumerated=false}

We will use dual-fitting to prove the competitive ratio. The dual we use
is as follows: for each timestep $t < A$ such that $x_W < 1$, set
$y_t = 1$.

First, we show that for each timestep $t < A$ such that $x_W < 1$, the
cost of the primal solution increases by at most $1 + \alpha W$. Let
$x_W(t)$ be the value of $x_W$ after the multiplicative update in
timestep $t$. The cost of the primal solution increases by

$$\begin{equation}
\label{eq-1}
(x_W(t) - x_W(t-1))W + x_t.
\end{equation}$$

We have that

$$\begin{equation}
\label{eq-2}
x_t = 1 - x_W(t) \leq 1 - x_W(t-1).
\end{equation}$$

Moreover, we have

$$\begin{equation}
\label{eq-3}
x_W(t) - x_W(t-1) = \frac{x_W(t-1)}{W} + \alpha.
\end{equation}$$

Plugging @eq-2 and @eq-3 into @eq-1, we get that the increase in the
cost of the primal solution is at most $1+\alpha W$.

Thus, the cost of the primal solution is at most $1+\alpha W$ times the
value of the dual solution. We now set $\alpha$ so that the dual
solution is feasible, which would imply a competitive ratio of
$1 + \alpha W$. The only constraint that we have to worry about is the
constraint $\sum_t y_t \leq W$. In particular, we need to set $\alpha$
so that $x_W(W) = 1$.

Observe that

$$\begin{align*}
x_W(W)
&= \left(1 + \frac{1}{W}\right)x_W(W-1) + \alpha \\
&= \left(1 + \frac{1}{W}\right)^2x_W(W-2) + \left(1 + \frac{1}{W}\right)\alpha + \alpha\\
&= \left(1+ \frac{1}{W}\right)^{W-1}\alpha + \ldots + \left(1 + \frac{1}{W}\right)\alpha + \alpha\\
&= \alpha \sum_{i=0}^{W-1} \left(1 + \frac{1}{W}\right)^i\\
&= \alpha W \left(\left(1+ \frac{1}{W}\right)^W  - 1\right),
\end{align*}$$

where the last equality follows by applying the [formula for geometric
series](https://en.wikipedia.org/wiki/Geometric_series#Definition:_finite_geometric_series).

Thus, we get a feasible dual solution and a competitive ratio of
$1 + \alpha W$ when we set $\alpha$ so that
$$\alpha W \left(\left(1+ \frac{1}{W}\right)^W  - 1\right) = 1,$$
which is equivalent to
$$\alpha W= \frac{1}{\left(1+ \frac{1}{W}\right)^W  - 1}.$$
This gives a competitive ratio of
$$1 + \alpha W = 1 + \frac{1}{\left(1+ \frac{1}{W}\right)^W  - 1} \approx \frac{e}{e-1}.$$

:::

Finally, we show how to round the solution online. The online rounding
algorithm is as follows: pick $\Theta \in [0,1]$ uniformly at random;
once $x_W$ is at least $\Theta$, walk.

We now analyze the expected cost of the algorithm. Let $X_W$ be the
random variable indicating whether the algorithm walked and $X_t$ be the
random variable indicating whether it waited at timestep $t$. By
linearity of expectation, the expected cost of the algorithm is
$$WE[X_W] + \sum_t E[X_t].$$
We have that $E[X_W] = \Pr[\Theta \leq x_W] = x_W$. At timestep $t$, the
algorithm waits if and only if $\Theta > x_W(t)$, and so
$$E[X_t] = \Pr[\Theta > x_W(t)] = 1 - x_W(t) = x_t,$$
by definition of the fractional algorithm. Thus, the expected cost of
the algorithm is at most $Wx_W + \sum_t x_t$ which is exactly the cost
of the fractional solution, and in turn is $\frac{e}{e-1}$ competitive.

Putting the online fractional algorithm together with the online
rounding algorithm gives a $\frac{e}{e-1}$ competitive randomized
algorithm for Online Tram Waiting.
