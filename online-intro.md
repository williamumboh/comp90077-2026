(sec-online-intro)=

# Introduction to Online Algorithms

So far, we have only considered algorithms in the traditional
computational model: the entire input is given to the algorithm, which
then performs some computation, and then outputs a solution. However,
many practical problems do not fit in this model.

For example, consider a ridesharing platform such as Uber. The platform
receives passenger ride requests over time and needs to quickly match
each request to an available driver, without precise knowledge of
when/where future requests will be.

We now turn our attention to *online algorithms*, algorithms that can
make decisions over time, without complete knowledge of the future. In
contrast, we call algorithms in the traditional model *offline
algorithms*.

## Rent or Buy Problem

The simplest online problem is the Rent or Buy Problem (aka Ski Rental).
Traditionally, the problem is described in the context of skiing.
However, since we are in Melbourne, we will describe it in the context
of waiting for a tram, and call it the *Tram Waiting Problem*.

Suppose we are at Melbourne Central and need to get to lecture. (See
figure below for an illustration.) The Google Maps says the tram takes 8
minutes and walking takes 21 minutes. Of course, we need to first wait
for the tram to arrive. If the tram arrives on schedule, then we only
need to wait 6 minutes, for a total of at most 14 minutes. Thus, waiting
for the tram is the optimal choice.

:::: {figure label=fig-maps}

::: {image width=95%} ./w11-maps.png

:::

To wait, or not to wait, that is the question.

::::

However, if tram services are delayed for some unknown amount of time,
how long should we wait before giving up on the tram and instead walk to
lecture?

:::: {figure}

::: {image width=35%} ./w11-delayed.png

:::

The bane of many a Melburnian's existance

::::

Let us now formalize the problem. We are given as input a positive
integer $W$ called the walking time. The tram arrives after $x$
timesteps (i.e. on timestep $x+1$), which is **unknown** to us. At each
timestep (1 minute per step), we see whether the tram has arrived or
not. If the tram has arrived, then we are happy. Otherwise, we need to
decide whether to wait for the next timestep, or to walk. The goal is to
minimize the cost: total waiting time plus walking time.

To keep things simple, we are going to assume that once we start
walking, we keep walking until we reach the destination. In other words,
at each timestep, if the tram does not arrive at that timestep, we
decide whether to wait for 1 minute or to walk for $W$ minutes. We will
also only count the time waiting for the tram, not the time on the tram.

Equivalently, the problem is as follows: given as input $W$, decide on a
threshold $t \geq 0$ (without knowledge of $x$) such that if the tram
has not arrived by timestep $t$, then we walk. In particular, the
algorithm's cost is $t+W$ if $t \leq W$ and $x$ otherwise.

::: {prf:example}

In the example in @fig-maps, $W = 21$. Suppose we set $t = 10$. If it
turns out that $x=5$, then our cost is $5$. On the other hand, if it
turns out that $x=15$, then we would have waited for $t = 10$ minutes
and then walked for $21$ minutes, for a total cost of $31$.

:::

Now that the task is clear, it remains to define what it means to
perform well. We will use the notion of *competitive analysis*.

(sec-competitive)=

### Competitive Analysis

::: {prf:definition label=def-competitive} Competitive Ratio

An *$α$-competitive algorithm* for an online problem $\Pi$ is one that
on every instance $I$ of $\Pi$, returns a solution whose cost is at most
$\alpha\cdot \operatorname{OPT}(I)$ (for minimization problems), or a
solution whose value is at least $\alpha\cdot\operatorname{OPT}(I)$ (for
maximization problems). Here, $\operatorname{OPT}(I)$ is the cost/value
of the offline optimal solution, i.e. the best solution given full
knowledge of the instance $I$.

The ratio $\alpha$ is the *competitive ratio* of the algorithm.

:::

An important feature of competitive analysis is that it makes no
assumption about the future, e.g. there is no historical data to train a
machine learning model one. Moreover, we are comparing against offline
algorithms, not online algorithms.

Thus, an algorithm that works well under competitive analysis works well
under all circumstances, even against adversarial input, where the input
is constructed by an adversary that knows the source code of the
algorithm, watches what the algorithm does, and then constructs the
input for the next timestep to make life as difficult for the algorithm
as possible.

This also suggests that it is useful to view an online problem as a
2-player game between the online algorithm and an adversary. In each
timestep, the adversary presents an input and the algorithm reacts to
the input. The adversary is able to choose the next input based on the
algorithm's previous reactions, and how the algorithm will react to each
possible input. See figure below for an illustration.

:::: {figure}

::: {image width=35%} ./online-book.png

:::

Book cover illustrating how the adversary constructs the next input.

::::

Let's apply competitive analysis to the Tram Waiting Problem. An
$α$-competitive algorithm is one that given $W$, finds a threshold $t$
such that for every $x$, the cost of the algorithm is at most $\alpha$
of the offline optimal solution.

Next, we need to get a handle on the offline optimal solution.

### Offline Algorithm

For the Tram Waiting Problem, an offline algorithm is given $W$ *and*
$x$, and then returns a threshold $t$ that is good for the given $x$. If
we know when the tram will arrive, then we should wait for the tram if
the waiting time is at most the walking time, and otherwise, we should
walk immediately at the first timestep. Thus, the cost of the offline
optimal solution is $\min\{x,W\}$.

::: {prf:lemma label=lem-rob-opt}

Given walking time $W$ and tram arrival time $x$, the cost of the
offline optimal solution is $\min\{x,W\}$.

:::

### Online Algorithm

What do we do if the tram arrival time $x$ is unknown? Intuitively, we
want to wait a little bit, but not too long. If we walk immediately,
then if $x=1$, our cost is $W$ but the optimal cost $\min\{x,W\}=1$. On
the other hand, if we never walk (i.e. $t = \infty)$, then our cost is
$x$, which can be much larger than $W$.

The greedy algorithm waits for as long as the walking time, and then
walks, i.e. its threshold is $t = W$.

::: {prf:theorem label=thm-greedy}

The greedy algorithm is a deterministic $2$-competitive algorithm for
the Online Tram Waiting Problem.

:::

We give two proofs, the first is by case analysis.

::: {prf:proof enumerated=false} Case analysis

Since $t=W$, the cost of greedy is $x$ when $x \leq W$ and $2W$ when
$x > W$. By @lem-rob-opt, the cost of greedy is at most twice the
offline optimal solution, in either case.

:::

The second proof uses a charging argument, and is in my opinion, more
insightful and generalizable to other problems.

::: {prf:proof enumerated=false} Charging argument

The idea is to charge the walking time of greedy to the waiting time of
greedy, which in turn is charged against $\operatorname{OPT}$.

First, observe that the algorithm only walks once it has waited for $W$
timesteps. In other words, its walking time is always at most its
waiting time. Thus, the total cost of the algorithm is at most twice its
waiting time.

Since $t=W$, its waiting time is at most $W$. Moreover, by definition of
the problem, every algorithm waits no longer than the tram arrival time,
so the waiting time of greedy is at most
$\min\{x,W\} = \operatorname{OPT}$. Thus, the cost of greedy is at most
$2\operatorname{OPT}$.

:::

### Deterministic Lower Bound

We now show that greedy is the best deterministic algorithm.

::: {prf:theorem label=thm-rob-lb}

Every deterministic algorithm for the Tram Waiting Problem has
competitive ratio at least 2.

:::

::: {prf:proof enumerated=false}

As discussed in @sec-competitive, we will use an adversarial argument:
the adversary sees the algorithm's threshold $t$ and then sets $x$ to
ensure that the algorithm's competitive ratio is at least 2. Consider a
deterministic algorithm. Fix a walking time $W$ and let $t$ be the
algorithm's threshold.

Intuitively, the worst case is when the tram arrives immediately after
we decide to walk, i.e. $x = t$. Indeed, when $x=t$, the algorithm's
total cost is $t + W$; on the other hand, by @lem-rob-opt, the optimal
offline cost is $\min\{x,W\} = \min\{t,W\}$. By an averaging argument,
we get that $\min\{t,W\} \leq (t+W)/2$.

:::

### Rent or Buy Conclusion

We have seen that greedy has competitive ratio 2 and this is the
best-possible among deterministic algorithm.

We briefly mention randomized algorithms. The proof for @thm-rob-lb
requires the adversary to know the precise threshold $t$. Thus, by
picking a *random* threshold, we gain some advantage over the adversary.

::: {prf:theorem label=thm-rob-rand}

There is a randomized algorithm with expected competitive ratio
$\frac{e}{e-1}=$ 1.58… for the Tram Waiting Problem.

:::

Finally, the Rent or Buy Problem captures a common phenomena occurring
in many other online problems and practical problems. In these problems,
we have two actions at each timestep:

- a "rent" action which is cheap but short-term
- a "buy" action which is expensive but long-term

One example of a practical problem is equipment maintenance. Suppose a
factory has some old equipment. Due to its age, it needs frequent
maintenance, say weekly. New equipment is expensive but requires much
less frequent maintenance. So, every week, the factory manager has to
decide whether to pay for maintenance or to pay more for new equipment.
The unknown variable here is how long the factory needs the equipment
for. For example, if the CEO decides to make other products, then the
equipment is retired and no longer needs mainenance.

## Online Bipartite Matching

In the Online Bipartite Matching problem, we are initially given the
left-hand side of a bipartite graph and we start with an empty matching
$M = \emptyset$. Then, in each timestep, a vertex $v$ arrives on the
right-hand side with some edges to the left-hand side, and we decide
whether to add one of $v$'s edges to $M$, subject to the constraint that
$M$ is always a matching. Our goal is maximize $|M|$.

For this problem, decisions are irrevocable:

- Once an edge is added to $M$, it cannot be removed from $M$.
- Once we decide not to add an edge to $M$, it is gone forever.
  Equivalently, we are not allowed to pick edges from vertices that
  arrived earlier.

Observe that while there is an [offline algorithm](#alg-bipartite) that
finds the maximum matching in bipartite graphs, it relies on [augmenting
paths](#def-augmenting), which in turn requires it to remove previously
added edges. Therefore, it is an inherently offline algorithm.

To motivate the irrevocable decisions model, consider the problem of ad
allocations on websites. We are given a set of ads that advertisers want
us to show to website visitors. Then, as visitors arrive over time, for
each visitor we need to decide which ad to show. Each ad has a target
demographic and we can only show the ad to a visitor in that
demographic, and each advertiser has a budget that limits how many times
the ad can be shown. Our goal is to maximize our revenue by showing as
many ads as we can. In this setting, once we decide to show an ad or
once a visitor has left, the decision is irrevocable.

While the greedy algorithm is not optimal in the offline setting, it
works in the online setting: when $v$ arrives, add any one of its edges
to $M$, if possible.

::: {prf:theorem label=thm-online-matching-greedy}

Greedy is a deterministic $(1/2)$ competitive algorithm for Online
Bipartite Matching.

:::

::: {prf:proof enumerated=false}

Since greedy adds an edge to its matching whenever it can, its matching
$M$ is a maximal matching. We can then use the fact that [every maximal
matching is a $(1/2)$ approximation](#thm-maximal-matching-approx) to
get the theorem.

:::

Can we do better? It turns out that there is no better deterministic
algorithm.

::: {prf:theorem label=thm-online-matching-lb}

Every deterministic algorithm has competitive ratio at most 1/2.

:::

::::: {prf:proof enumerated=false}

Fix a deterministic algorithm. Consider a graph with 2 vertices
$u_1, u_2$ on the left-hand side. We use an adversarial argument to
construct the right-hand side vertices. The first right-hand side vertex
$v_1$ has edges to $u_1$ and $u_2$.

:::: {figure}

::: {image width=35%} ./w11-matching-lb-1.png

:::

Illustration of the first arrival.

::::

If the algorithm does not pick any edge, then no further right-hand side
vertices arrive. The optimal solution is to pick one of the edges while
the algorithm picks none. Thus, to have a finite competitive ratio, the
algorithm must pick one of the two edges. The next vertex $v_2$ has a
single edge to the vertex incident to the algorithm's edge.

If the algorithm picks the edge to $u_2$, then $v_2$ only has an edge to
$u_2$. So, the optimal solution picks $(u_1,v_1)$ and $(u_2,v_2)$.

:::: {figure}

::: {image width=35%} ./w11-matching-lb-4.png

:::

The blue edge is the single edge in the algorithm's matching while the
red ones are the edges in the optimal matching.

::::

On the other hand, if the algorithm picks the edge to $u_1$, then the
next vertex $v_2$ only has an edge to $u_1$. So, the optimal solution
picks $(u_2,v_1)$ and $(u_1,v_2)$.

:::: {figure}

::: {image width=35%} ./w11-matching-lb-3.png

:::

The blue edge is the single edge in the algorithm's matching while the
red ones are the edges in the optimal matching.

::::

In both cases, the algorithm cannot pick the edge incident to $v_2$ and
so has competitive ratio of $1/2$.

:::::
