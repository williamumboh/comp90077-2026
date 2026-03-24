(sec-tut3)=

# Tutorial 3

::: {exercise label=ex-3-1}

Consider the [greedy algorithm](#alg-submodular-greedy) for
cardinality-constrained submodular maximization. The algorithm and
analysis assumes that we are able to efficiently find the element
$e \notin I$ that maximizes $g(I \cup \{e\}) - g(I)$. In some
applications, this may not be possible.

Define the $c$-greedy algorithm to be one that is only able to
approximate this step, i.e. it adds an element $e' \notin I$ such that
$$g(I \cup \{e'\}) - g(I) \geq c \cdot \max_{e \notin I} (g(I \cup \{e\}) - g(I)),$$
for some $c < 1$. Analyze the approximation factor of the $c$-greedy
algorithm by going through the [analysis](#prf-submod-max) of the
original greedy algorithm and see what changes.

:::

::: {solution} ex-3-1

The analysis is mostly the same as the [analysis](#prf-submod-max) of
the original greedy algorithm, except that in each iteration $\ell$, we
only find an element $\tilde{e}$ such that
$$g(I_{\ell-1} \cup \{\tilde{e}\})- g(I_{\ell-1})
\geq \frac{c(g(I^*) - g(I_{\ell-1}))}{k},$$ and thus, the gap between
$g(I_\ell)$ and $g(I^*)$ decreases by a smaller factor of $(1-c/k)$.

Overall, we get $$g(I_k)
&\geq \left(1 - \left( 1 - \frac{c}{k}\right)^k\right) g(I^*) \\
&\geq \left((1 - \left(\frac{1}{e}\right)^{-c}\right) g(I^*).$$ (Note
that $e$ here is the base of the natural logarithm, not an element.)

Thus, the approximation ratio is $1 - e^{-c}$.[^1]

:::

::: {exercise label=ex-3-2}

Show that the two definitions of [submodularity](#def-submodular) are
equivalent. That is, given a set function
$g: 2^E \rightarrow \mathbb{R}$, show that for every
$S \subseteq T \subseteq E$,
$$g(S \cup \{e\}) - g(S) \geq g(T \cup \{e\}) - g(T)$$ if and only if
for every $S,T \subseteq E$,
$$g(S) + g(T) \geq g(S \cap T) + g(S \cup T).$$

:::

::: {hint}

The easier direction is showing that the second definition implies the
first.

:::

::: {hint class=dropdown}

For both directions, it helps to first rearrange the second inequality
$$g(S) + g(T) \geq g(S \cap T) + g(S \cup T)$$ to get
$$g(S) - g(S \cap T) \geq g(S \cup T) - g(T).$$

:::

::: {solution class=dropdown} ex-3-2

By rearranging, the second inequality is equivalent to
$$g(S) - g(S \cap T) \geq g(S \cup T) - g(T).$$

We begin by showing that the second property implies the first
(decreasing marginals).

Suppose $X \subseteq Y$ and $e \notin Y$. We now apply the previous
inequality with a suitable setting of $S$ and $T$ to show that
$$g(X \cup \{e\}) - g(X) \geq g(Y \cup \{e\}) - g(Y).$$

Matching up the terms of the two inequalities suggests setting
$S = X \cup \{e\}$ and $T = Y$. Let us now check that this actually
works. We have that $S \cap T = X$ since $X$ is contained in $Y$ and
$e \notin Y$. We also have that $S \cup T = Y \cup \{e\}$ since $X$ is
contained in $Y$.

Thus, we conclude that setting $S = X \cup \{e\}$ and $T = Y$ in
Inequality (7) gives us Inequality (8).

:::

::: {hint class=dropdown} Hint for first implies second

After rearranging the second inequality as above hint, we want to show
that the decreasing marginals property implies
$$g(S) - g(S \cap T) \geq g(S \cup T) - g(T).$$

Next, express the difference $g(S) - g(S \cap T)$ as a sum of marginals
as in the [proof](#prf-submod-max) that greedy is a
$(1-1/e)$-approximation for cardinality-constrained submodular
maximization. Do the same for $g(S \cup T) - g(T)$.

You should now be able to apply the decreasing marginals property.

:::

::: {solution class=dropdown} ex-3-2

We now turn to showing that if for every $X \subseteq Y$ and
$e \notin Y$, we have
$$g(X \cup \{e\}) - g(X) \geq g(Y \cup \{e\}) - g(Y),$$ then for every
$S, T \subseteq E$, we have
$$g(S) - g(S \cap T) \geq g(S \cup T) - g(T).$$

Since the decreasing marginals property is about the marginal when
adding an element to a set versus adding to a superset, we want to first
express $g(S) - g(S \cap T)$ and $g(S \cup T) - g(T)$ as a sum of
marginals. Observe that
$$S \setminus (S \cap T) = S \setminus T = (S \cup T) \setminus T.$$ In
other words, adding $S \setminus T$ to $S \cap T$ and $T$ gives us $S$
and $S \cup T$, respectively.

Suppose the elements of $S \setminus T$ are $e_1, \ldots, e_k$. Define
$S_0 = S \cap T$ and $S_i = S_0 \cup \{e_1, \ldots, e_i\}$. Similarly,
define $T_0 = T$ and $T_i = T_0 \cup \{e_1, \ldots, e_i\}$. Note that
$S_k = S$ and $T_k = S \cup T$, and for each $i$, we have
$S_i \subseteq T_i$ since $S \cap T$ is contained in $T$.

We can now express $g(S) - g(S \cap T)$ and $g(S \cup T) - g(T)$ as a
sum of marginals: $$g(S) - g(S \cap T)
&= \sum_{i=1}^k g(S_i) - g(S_{i-1}) \\
&= \sum_{i=1}^k g(S_{i-1} \cup \{e_i\}) - g(S_{i-1})$$ and
$$g(S \cup T) - g(T)
&= \sum_{i=1}^k g(T_i) - g(T_{i-1}) \\
&= \sum_{i=1}^k g(T_{i-1} \cup \{e_i\}) - g(T_{i-1}).$$

For each $i$, we have $S_{i-1} \subseteq T_{i-1}$ and so the decreasing
marginals property imply that
$$g(S_{i-1} \cup \{e_i\}) - g(S_{i-1}) \geq g(T_{i-1} \cup \{e_i\}) - g(T_{i-1}).$$

Putting all of the above together, we have $$g(S) - g(S\cap T)
&= \sum_{i=1}^k g(S_{i-1} \cup \{e_i\}) - g(S_{i-1}) \\
&\geq \sum_{i=1}^k  g(T_{i-1} \cup \{e_i\}) - g(T_{i-1})\\
&= g(S \cup T) - g(T),$$ as desired.

:::

::: {exercise label=ex-3-3}

Show that the [matroid rank function](#def-matroid-rank) is submodular.

:::

The following solution is interesting because it uses the second
definition of submodularity while previously, we've only used the
decreasing marginals property.[^2]

::: {solution class=dropdown} ex-3-3

Let $M$ be a matroid with ground set $E$ and $r$ be its rank function.
We will show that for every $S,T \subseteq E$, we have
$$r(S) + r(T) \geq r(S \cap T) + r(S \cup T).$$

Let $S, T \subseteq E$. Suppose $C$ is the maximally independent set in
$S \cap T$. Using the [augmentation property](#def-matroid) of matroids,
we get that there is a maximally independent set $B$ of $S \cup T$ that
contains $C$. By definition, we have $r(S \cap T) = |C|$ and
$r(S \cup T) = |B|$.

Observe that $S \cup T$ can be partitioned into 3 parts:
$S \setminus T$, $S \cap T$ and $T \setminus S$. We now partition $B$
into 3 parts as well:

- $B_S = B \cap (S \setminus T)$,
- $C = B \cap (S \cap T)$,
- $B_T = B \cap (T \setminus S)$.

Thus, we have $|B| = |B_S| + |C| + |B_T|$ and so
$$r(S \cap T) + r(S \cup T) = |B_S| + |C| + |B_T| + |C|.$$

On the other hand, by the [downwards-closed property](#def-matroid) of
matroids, $B_S \cup C$ is independent since $B_S \cup C$ is contained in
$B$. Since $B_S$ and $C$ are both contained in $S$, we have that
$B_S \cup C$ is also contained in $S$, and thus,
$r(S) \geq |B_S| + |C|$. Similarly, $r(T) \geq |B_T| + |C|$.

Putting the above together, we get $$r(S) + r(T)
&\geq |B_S| + |C| + |B_T| + |C|\\
&= r(S \cap T) + r(S\cup T).$$

:::

[^1]: As a sanity check, note that as $c$ becomes smaller, the
    approximation ratio also becomes smaller, as we'd expect. (Remember
    that smaller is worse.)

[^2]: Can you find a proof using the decreasing marginals property?
