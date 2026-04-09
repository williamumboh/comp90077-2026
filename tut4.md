(sec-tut4)=

# Tutorial 4

In this tutorial, we will make more precise the argument for the
following theorem.

::: {prf:theorem}

If there exists a constant[^1] $\alpha > 0$ such that there is a
$(1-1/e+α)$-approximation algorithm ALG for [Coverage
Maximization](#def-coverage-max), then there exists a
$(1-\epsilon) \ln n$ approximation algorithm for [Set
Cover](#prob-set-cover), for some $\epsilon >0$ that depends only on
$\alpha$.

:::

In the following, we will consider a fixed instance of Set Cover.

::: {exercise label=ex-4-1}

Let $I^*$ be the optimal solution. Suppose we know $k^*=|I^*|$. Show how
to use this knowledge and ALG to get a solution $I$ with
$|I| \leq ((1-\epsilon)\ln n)k^*$, for some $\epsilon > 0$ that depends
only on $\alpha$.

:::

::: {solution class=dropdown label=sol-4-1} ex-4-1

Consider the following algorithm:

- Initialize $I = \emptyset$.
- **while** not all elements are covered by $I$ **do**
  - Let $U$ be the set of elements not covered by $I$
  - Use ALG to find an $(1-1/e+\alpha)$ approximation $J$ to the problem
    of finding $k^*$ sets that cover as many uncovered elements as
    possible
  - Add $J$ to $I$
- **end while**
- **return** $I$

Since each iteration adds $k^*$ sets to $I$, it suffices to bound the
number of iterations. Let $U_i$ be the set of uncovered elements at the
end of the $i$-th iteration. ALG guarantees that $J$ covers at least
$(1-1/e+α)$-fraction of $U_i$ and so
$$| U_{i+1} |
≤ \left(\frac{1}{e} - α\right)|U_i|
≤ \left(\frac{1}{e} - α\right)^{i+1} n.$$

Thus, the number of uncovered elements decrease by a factor of
$e/(1-\alpha\cdot e)$ after each iteration and so every element is
covered after $$\log_{e/(1- \alpha\cdot e)} n$$ iterations.

Applying change of base rule for logarithms, we have
$$\log_{e/(1- \alpha\cdot e)} n
&= \frac{\ln n}{\ln \frac{e}{1-\alpha \cdot e}}\\
&= \frac{\ln n}{1 + \ln \frac{1}{1-\alpha \cdot e}}\\
&\leq \frac{\ln n}{1 + \alpha \cdot e},$$
where the last inequality follows from the fact[^2] that
$\ln \frac{1}{1-x} \geq x$. Finally, observe that
$$\frac{1}{1 + \alpha \cdot e}
= 1 - \frac{\alpha\cdot e}{1+ \alpha\cdot e}
= 1 - \epsilon$$
where $\epsilon = \frac{\alpha\cdot e}{1+ \alpha\cdot e}$.

:::

::: {exercise label=ex-4-2}

Modify your algorithm from the previous exercise so that it still works
without knowledge of $k^*$.

:::

::: {hint class=dropdown}

How many possibilities are there for $k^*$? Can we somehow try all
possibilities?

:::

::: {solution class=dropdown} ex-4-2

Since $1 \leq k^* \leq n$, we run the algorithm from @sol-4-1 for $k=1$
up to $k = n$ and output the smallest set cover from each of these runs.
The set cover from the run when $k = k^*$ has size at most
$((1−\epsilon)\ln n)k^*$, and thus the smallest set cover has size at
most $((1−\epsilon)\ln n)k^*$.

:::

[^1]: It is not a function of $n$, or $m$.

[^2]: Taylor series approximation.
