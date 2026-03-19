(sec-tut3)=

# Tutorial 3

::: {exercise label=ex-3-1}

Show that the two definitions of \[\[#def-submodular\]\[submodularity\]
are equivalent. That is, given a set function
$g: 2^E \rightarrow \mathbb{R}$, show that for every
$S \subseteq T \subseteq E$,
$$g(S \cup \{e\}) - g(S) \geq g(T \cup \{e\}) - g(T)$$ if and only if
for every $S,T \subseteq E$,
$$g(S) + g(T) \geq g(S \cap T) + g(S \cup T).$$

:::

::: {exercise label=ex-3-2}

Show that the [matroid rank function](#def-matroid-rank) is submodular.

:::

::: {exercise label=ex-3-3}

Consider the [greedy algorithm](#alg-submodular-greedy) for
cardinality-constrained submodular maximization. The algorithm and
analysis assumes that we are able to efficiently find the element
$e \notin I$ that maximizes $g(I \cup \{e\}) - g(I)$. In some
applications, this may not be possible.

Define the $c$-greedy algorithm to be one that is only able to
approximate this step, i.e. it adds an element $e' \notin I$ such that
$$g(I \cup \{e'\}) - g(I) \geq c \cdot \max_{e \notin I} (g(I \cup \{e\}) - g(I)),$$
for some $c < 1$. Analyze the approximation factor of the $c$-greedy
algorithm by going through the analysis of the original greedy algorithm
and see what changes.

:::
