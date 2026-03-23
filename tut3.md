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

::: {exercise label=ex-3-3}

Show that the [matroid rank function](#def-matroid-rank) is submodular.

:::
