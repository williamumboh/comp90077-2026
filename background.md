(sec-background)=

# Background

Refer to this section for a refresher on notation and terminology that
will be heavily used throughout. We assume that you have either seen
them before in other subjects or are able to spend extra time to quickly
get comfortable with them. For more detailed discussion, see the
relevant sections of:

- [Discrete Math
  Background](https://complexityincs.com/discrete-math.pdf) by Thomas
  Watson;
- [Mathematics for Computer
  Science](https://courses.csail.mit.edu/6.042/spring18/mcs.pdf) by Eric
  Lehman, F. Thomson Leighton, and Albert R. Meyer ([lecture
  slides](https://opencw.aprende.org/courses/electrical-engineering-and-computer-science/6-042j-mathematics-for-computer-science-spring-2015/lecture-slides/)).

(sec-background-graphs)=

## Graphs

Let $G = (V,E)$ be a graph with vertex set $V$ and edge set $E$. We
typically use $n$ to mean the number of vertices $|V|$ and $m$ to mean
the number of edges $|E|$.

Given an edge $e = (u,v)$, we say that vertices $u$ and $v$ are
*endpoints* of $e$. We say that a vertex $u$ is *incident* to an edge
$e$ if $u$ is an endpoint of $e$, and vice versa. We also say that $u$
is incident to a subset $F$ of edges if it is incident to one of them.

Intuitively, a path lets us go from one vertex to another by following
edges, and a cycle has an additional edge that lets us go back to the
first vertex. Formally, a *path* $P$ consists of a sequence of distinct
vertices $v_1, v_2, \ldots, v_k$ for some $k > 1$ and edges
$(v_1,v_2), (v_2, v_3), \ldots, (v_{k-1}, v_k)$. We also use $P$ to
denote the set of edges on the path. A *cycle* consists of a path plus
an additional edge $(v_k, v_1)$ that closes the path.

Let $F$ be a subset of edges. We say that $F$ *connects* vertices $u$
and $v$ if $F$ contains a path between $u$ and $v$, and *spans* a subset
of vertices $X$ if it connects all vertices of $X$. Moreover, we say
that $F$ is:

- *spanning* if it connects all of $V$;
- *acyclic* if it contains no cycles;
- *connected* if for every pair of vertices $u$ and $v$ that are
  incident to $F$, there is a $u$-$v$ path contained in $F$;
- a *forest* if it is acyclic;
- a *tree* if it is connected ****and**** acyclic;

Let $X$ be a subset of vertices. We say that $X$ is a *connected
component* (or *component*) of $G$ if $E$ spans $X$ and there is no
other vertex $v$ such that $E$ spans $X \cup \{v\}$ as well.

(sec-background-sets)=

## Set notation

In the following, let $S$ and $T$ be sets.

- $\emptyset$ is the empty set
- $x \in S$ means $x$ is in $S$; and $x \notin S$ means $x$ is not in
  $S$.
- $S \setminus T$ is the set of elements in $S$ but not in $T$
- $S \cap T$ is the set of elements in $S$ **and** $T$, and is called
  the *intersection* of $S$ and $T$
- $S \cup T$ is the set of elements in $S$ **or** $T$, and is called the
  *union* of $S$ and $T$
- $S \subseteq T$ means $S$ is a subset of $T$, i.e. every element of
  $S$ is an element of $T$; we also say that $S$ is *contained in* $T$.
  Moreover, we say that $S$ is a *proper subset* of $T$ (or $S$ is
  *properly contained in* $T$) if $S$ is contained in $T$ and $T$
  contains some element not in $S$; this is written as $S \subsetneq T$.
- $S \supseteq T$ means $S$ is a superset of $T$, i.e. every element of
  $T$ is an element of $S$; we also say that $S$ *contains* $T$.
  Similarly, we say that $S$ is a *proper superset* of $T$ (or $S$
  *properly contains* $T$) if $S$ contains $T$ and $S$ contains some
  element not in $T$; this is written as $S \supsetneq T$.
- $2^S$ is the set containing every subset of $S$ (including $\emptyset$
  and $S$), and is called the *powerset* of $S$. Sometimes, it is
  denoted by $\mathcal{P}(S)$.
  - For example, when $S$ is the set $\{a,b\}$, then the elements of
    $2^S$ are $\emptyset$, $\{a\}$, $\{b\}$ and $\{a,b\}$.
- $\{x \in S \mid P(x)\}$ means the set of elements in $S$ that
  satisfies the predicate $P$. This is called the *set-builder
  notation*.
  - For example, if $S$ is a set of numbers, then
    $\{x \in S \mid x \text{ is even}\}$ is the set of even numbers in
    $S$.
  - Sometimes, : is used in place of $\mid$. For example,
    $\{x \in S : x \text{ is even}\}$

For a more in-depth and beginner-friendly discussion see also [Guide to
Elements and
Subsets](https://web.stanford.edu/class/archive/cs/cs103/cs103.1246/resources/Guide%20to%20Elements%20and%20Subsets.pdf).

(sec-background-thms)=

## Theorems and Lemmas

Usually, a *theorem* is an important result that we want to show, and a
*lemma* is a smaller result that is not necessarily interesting by
itself, but is useful to bigger results. Often, a theorem is proved by
proving a sequence of smaller lemmas. In a way, lemmas are similar to
the role of subroutines in programming.

See also [CS103 Guide to
Proofs](https://web.stanford.edu/class/cs103/guide_to_proofs_on_discrete_structures#writing-longer-proofs)
and Section 1.3.2 of [Introduction to Theoretical Computer
Science](https://introtcs.org/public/lec_00_1_math_background.html).
