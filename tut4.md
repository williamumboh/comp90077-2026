(sec-tut4)=

# Tutorial 4

In this tutorial, we will make more precise the argument for the
following theorem.

::: {prf:theorem}

If there exists a constant[^1] $\alpha > 0$ such that there is a
$(1-1/e+α)$-approximation algorithm ALG for [Coverage
Maximization](#def-coverage-max), then there exists a
$(1-ε)ln n$-approximation algorithm for [Set Cover](#prob-set-cover),
where $\epsilon >0$ and depends only on $\alpha$.

:::

In the following, we will consider a fixed instance of Set Cover.

::: {exercise}

Let $I^*$ be the optimal solution. Suppose we know $k^*=|I^*|$. Show how
to use this knowledge and ALG to get a solution $I$ with \$\|I\| ≤
((1-ε)ln n)k^\*^, for some $\epsilon > 0$ that depends only on $\alpha$.

:::

::: {exercise}

Modify your algorithm from the previous exercise so that it still works
without knowledge of $k^*$.

:::

::: {hint class=dropdown}

How many possibilities are there for $k^*$? Can we somehow try all
possibilities?

:::

[^1]: It is not a function of $n$, or \$m\$
